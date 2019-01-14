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

 ## INSTALLATION
1 Ubuntu Xenial 16.04 (LTS)
2 CentOS 7.4
- Make sure the OS version is matched before continuing. 
- *Every node has a unique MAC address and product UUID*: Some plugins use the MAC address or product UUID as a unique machine ID to identify nodes (for example, kube-dns). If they are duplicated in the cluster, kubeadm may not work while starting the plugin:
// check MAC address of your NIC
$ ifconfig -a
// check the product UUID on your host
$ sudo cat /sys/class/dmi/id/product_uuid
- Every node has a different hostname: If the hostname is duplicated, the Kubernetes system may collect logs or statuses from multiple nodes into the same one.
- *Docker is installed* As mentioned previously, the Kubernetes master will run its daemon as a container, and every node in the cluster should get Docker installed. For how to perform the Docker installation, you can follow the steps on the official website: (Ubuntu: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/, and CentOS: https://docs.docker.com/engine/installation/linux/docker-ce/centos/) Here we have Docker CE 17.06 installed on our machines; however, only Docker versions 1.11.2 to 1.13.1, and 17.03.x are verified with Kubernetes version 1.10.
Network ports are available: The Kubernetes system services need network ports for communication. The ports in the following table should now be occupied according to the role of the node:
Node role	Ports	System service
Master	|6443	Kubernetes API server
10248/10250/10255	kubelet local healthz endpoint/Kubelet API/Heapster (read-only)
10251	kube-scheduler
10252	kube-controller-manager
10249/10256	kube-proxy
2379/2380	etcd client/etcd server communication
Node	| 10250/10255	Kubelet API/Heapster (read-only)
30000~32767	Port range reserved for exposing container service to outside world

The Linux command, netstat, can help to check if the port is in use or not:

// list every listening port
$ sudo netstat -tulpn | grep LISTEN
Network tool packages are installed. ethtool and ebtables are two required utilities for kubeadm. They can be download and installed by theapt-get or yumpackage managing tools.
