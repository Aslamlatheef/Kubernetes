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

### kubelet
kubelet is the main process on the Kubernetes node that communicates with the Kubernetes master to handle the following operations:

- Periodically accesses the API controller to check and report
- Performs container operations
- Runs the HTTP server to provide simple APIs

### Kube Proxy (kube-proxy)
The proxy handles the network proxy and load balancer for each container. It changes Linux iptables rules (nat table) to control TCP and UDP packets across the containers.

After starting the kube-proxy daemon, it configures iptables rules; you can use iptables -t nat -L or iptables -t nat -S to check the nat table rule

### etcd
etcd (https://coreos.com/etcd/) is the distributed key-value data store. It can be accessed via the RESTful API to perform CRUD operations over the network. Kubernetes uses etcd as the main data store.

You can explore the Kubernetes configuration and status in etcd (/registry) using the curl command

### Kubernetes network
Network communication between containers is the most difficult part. Because Kubernetes manages multiple nodes (hosts) running several containers, those containers on different nodes may need to communicate with each other.

If the container's network communication is only within a single node, you can use Docker network or Docker compose to discover the peer. However, along with multiple nodes, Kubernetes uses an overlay network or container network interface (CNI) to achieve multiple container communication.

