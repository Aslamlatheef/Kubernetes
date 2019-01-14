## Kubernetes Cook Book 
Kubernetes is made up of the following components:

Kubernetes master
Kubernetes nodes
etcd
Kubernetes network
These components are connected via a network, as shown in the following diagram:




Kubernetes master: It connects to etcd via HTTP or HTTPS to store the data
Kubernetes nodes: It connect to the Kubernetes master via HTTP or HTTPS to get a command and report the status
Kubernetes network: It L2, L3 or overlay make a connection of their container applications
