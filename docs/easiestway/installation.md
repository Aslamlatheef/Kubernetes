### installation of Kubernetes 
--------------------------------------------------------------------------------------------------

## Install docker 

the version that i have used is UBUNTU 18.04 

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt-get update

sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu

sudo apt-mark hold docker-ce

### KUBE ADM INSTALL
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update

sudo apt-get install -y kubelet=1.12.7-00 kubeadm=1.12.7-00 kubectl=1.12.7-00

sudo apt-mark hold kubelet kubeadm kubectl

###
## create YAML file and create pod with kubectl

cat << EOF | kubectl create -f -

apiVersion: v1

  kind: Pod

metadata:

  name: nginx

spec:

containers:

    - name: nginx
      
      image: nginx
EOF

## Get a list of pods and verify that your new nginx pod is in the Running state

kubectl get pods

## Get more information about your nginx pod

kubectl describe pod nginx

## Delete the pod:

kubectl delete pod nginx
