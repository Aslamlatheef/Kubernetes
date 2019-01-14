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

### Scheduler (kube-scheduler)
The scheduler helps to choose which container runs on which nodes. It is a simple algorithm that defines the priority for dispatching and binding containers to nodes. For example:

- CPU
- Memory
- How many containers are running?

### Controller manager (kube-controller-manager)
The controller manager performs cluster operations. For example:

Manages Kubernetes nodes
Creates and updates the Kubernetes internal information
Attempts to change the current status to the desired status

### Command-line interface (kubectl)
After you install the Kubernetes master, you can use the Kubernetes command-line interface, kubectl, to control the Kubernetes cluster. For example, kubectl get cs returns the status of each component. Also, kubectl get nodes returns a list of Kubernetes nodes:

(full reference guide)[https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands] 

### Kubernetes node
The Kubernetes node is a slave node in the Kubernetes cluster. It is controlled by the Kubernetes master to run container applications using Docker (http://docker.com) or rkt (http://coreos.com/rkt/docs/latest/). In this book, we will use the Docker container runtime as the default engine.

Node or slave?
The term slave is used in the computer industry to represent the cluster worker node; however, it is also associated with discrimination. The Kubernetes project uses minion in the early version and node in the current version.

The following diagram displays the role and tasks of daemon processes in the node:
![alt text](https://github.com/Aslamlatheef/Kubernetes/blob/master/Images/images.png)


