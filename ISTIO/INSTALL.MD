

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
