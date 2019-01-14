# Kubernetes Cook Book 
### Kubernetes is made up of the following components:

- Kubernetes master 

- Kubernetes nodes

- etcd
![alt text](https://github.com/Aslamlatheef/Kubernetes/blob/master/Images/kubernetes-architecture.png)
- Kubernetes network

Kubernetes master: It connects to etcd via HTTP or HTTPS to store the data

Kubernetes nodes: It connect to the Kubernetes master via HTTP or HTTPS to get a command and report the status

Kubernetes network: It L2, L3 or overlay make a connection of their container applications

### Kubernetes master
The Kubernetes master is the main component of the Kubernetes cluster. It serves several functionalities, such as the following:

1 Authorization and authentication
2 RESTful API entry point
3 Container deployment scheduler to Kubernetes nodes
4 Scaling and replicating controllers
5 Reading the configuration to set up a cluster

![alt text](https://github.com/Aslamlatheef/Kubernetes/blob/master/Images/d05d8b65-158f-4a4b-8a4c-b9cf00ef1133.jpg)


There are several daemon processes that form the Kubernetes master's functionality, such as kube-apiserver, kube-scheduler and kube-controller-manager. Hypercube, the wrapper binary, can launch all these daemons.

In addition, the Kubernetes command-line interface, kubect can control the Kubernetes master functionality.

### API server (kube-apiserver)
The API server provides an HTTP- or HTTPS-based RESTful API, which is the hub between Kubernetes components, such as kubectl, the scheduler, the replication controller, the etcd data store, the kubelet and kube-proxy, which runs on Kubernetes nodes, and so on.
