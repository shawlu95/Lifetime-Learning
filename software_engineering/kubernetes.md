

#### Container

* encapsulated lightweight runtime environments for application module
* improve server utilization
* container runs container image
* encapsulate microservices and their dependencies

#### Container Orchestrator 

* connect different nodes
* a fault-tolerant and scalable solution
* designed for managing hundreds and thousands of containers running on a global infrastructure

#### Container image

* bundle application code and runtime, libraries, dependencies

#### Master Node

* **master node** provides a running environment for the **control plane** responsible for managing the state of a Kubernetes cluster
* has replicas which sync with the control plane
* **etcd** is a distributed key-value store which only holds cluster state related data
* **API server** the only component talking to **etcd**
* **kube-scheduler** is to assign new workload objects, such as pods, to nodes.

#### Pod

* A Pod is the smallest scheduling unit in Kubernetes.
* It is a logical collection of one or more containers scheduled together
* the collection can be started, stopped, or rescheduled as a single unit of work. 

#### Worker Node

* container runtime: docker, cri-o, containerd, frakti
* node agent - kubelet: connects to API server on master node
* **kube-proxy** is the network agent which runs on each node responsible for dynamic updates and maintenance of all networking rules on the node

