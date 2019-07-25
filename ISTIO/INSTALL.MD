# ISTIO ARCHITECTURE
## Core Components
### Envoy

Envoy is an open-source extension and service proxy provider, built for cloud-extensive meshes. The Istio mesh creates an extendible proxy system through Envoy.

### Mixer

The mixer is a part of the service mesh that helps in enforcing safety protocols, allowing access controls and implementing usage policies and works independently from the mesh.

### Pilot

Pilot provides all services for the Istio Envoy sidecars and allows for a more coherent traffic management system with high level routing.


## ISTIO WITH DOCKER

### Add current user to docker group
```
sudo usermod -aG docker cloud_user
```

### Install docker-compose and make it executable

```

sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-Linux-x86_64" -o /usr/local/bin/docker-compose  

sudo chmod +x /usr/local/bin/docker-compose

```

### Download Istio and unpack it

```

wget https://github.com/istio/istio/releases/download/1.0.6/istio-1.0.6-linux.tar.gz

tar -xvf istio-1.0.6-linux.tar.gz

```

### Preconfigure kubectl for pilot

```

kubectl config set-context istio --cluster=istio

kubectl config set-cluster istio --server=http://localhost:8080

kubectl config use-context istio

```

### Create a DOCKER_GATEWAY environment variable

```
export DOCKER_GATEWAY=172.28.0.1:
```

### Bring up Istio's control plane
```
docker-compose -f install/consul/istio.yaml up -d
```

### Change bookinfo.yaml to set port 30080 in place of port 9081
```
sed -i 's/9081/30080/' ./istio-1.0.6/samples/bookinfo/platform/consul/bookinfo.yaml
`
```

### Bring up the application
```
docker-compose -f ./istio-1.0.6/samples/bookinfo/platform/consul/bookinfo.yaml up -d
```

### Bring up the sidecars
```
docker-compose -f ./istio-1.0.6/samples/bookinfo/platform/consul/bookinfo.sidecars.yaml up -d
```

## Installing Istio on kubernetes

since the version are keep on changing, please refer below for latest installation document 
[installation of istio on kubernetes](https://istio.io/docs/setup/kubernetes/)

Add istioctl to our path:
```
export PATH=$PWD/istio-1.0.6/bin:$PATH
```
Set Istio to NodePort type, and set the nodePort to port 30080:
```
sed -i 's/LoadBalancer/NodePort/;s/31380/30080/' ./istio-1.0.6/install/kubernetes/istio-demo.yaml
```
Bring up the Istio control plane:
```
kubectl apply -f ./istio-1.0.6/install/kubernetes/istio-demo.yaml
```
Verify that the control plane is running, but note that it may take a minute or two:
```
kubectl -n istio-system get pods
```
Install the bookinfo application with manual sidecar injection:
```
kubectl apply -f <(istioctl kube-inject -f istio-1.0.6/samples/bookinfo/platform/kube/bookinfo.yaml)
```
Verify that the application is running and that there are 2 containers per pod. This may take another minute or so:
```
kubectl get pods
```
Create an ingress and virtual service for the application:
```
kubectl apply -f istio-1.0.6/samples/bookinfo/networking/bookinfo-gateway.yaml
```
Now, in a web browser, let's verify the page loads at the URL http://<kn1PUBLICIP ADDRESS>:30080/productpage.

Configure the Forwarder, Install Nginx, and Access the Grafana Dashboard

Configure Grafana to forward to localhost:
```
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &
```

Install and configure Nginx to proxy to the Grafana port:
```
sudo apt install -y nginx
```
Change the Nginx config by editing /etc/nginx/sites-enabled/default with whichever text editor you like (but remember privileges, and run something like sudo vi...). Comment out all existing lines in the location / directive, and add proxy_pass http://127.0.0.1:3000. The results should look like this:
```
location / {
    # First attempt to serve request as file, then
    # as directory, then fall back to displaying a 404.
    #try_files $uri $uri/ =404;
    proxy_pass http://127.0.0.1:3000;
}
```
Then restart Nginx:
```
sudo systemctl restart nginx
```
Load Grafana at http://<kmPUBLICIP ADDRESS/dashboard/db/istio-mesh-dashboard and review the installed Istio dashboards.

If the mesh traffic dashboard does not populate when the application is refreshed, then rerun this command to re-apply the mixer configuration:
```
kubectl apply -f ./istio-1.0.6/install/kubernetes/istio-demo.yaml
```

## istio logging

This is the Nginx configuration, located at /etc/nginx/sites-enabled/default:
```
server {
listen 80 default_server;
listen [::]:80 default_server;
root /var/www/html;
index index.html index.htm index.nginx-debian.html;
server_name _;  
#location / {proxy_pass  http://127.0.0.1:9090;}
location / {proxy_pass  http://127.0.0.1:3000;}
}
```
There are also 2 commands that are used to forward the ports.

The command to forward the ports for Prometheus:
```
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=prometheus -o jsonpath='{.items[0].metadata.name}') 9090:9090 &
```
The command the forward the port for Grafana:
```
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &
```
