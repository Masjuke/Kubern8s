
### Dashboard Component

### Cluster

  K8s cluster is made of master node and worker nodes, Collection of several Physical or Virtual machine. 1 set as master node of cluster 
  and others set as worker nodes and run distributed application container on multiple nodes.
  
  K8s cluster function is to manage orchestration of container application environment such
  - Deploy container application
  - Scheduling container
  - Scale Application
  - Expose services and load balancing application
  - Logging & Monitoring
  - Manage network connection between containers
  - Replications
  - Failover application
	- Auto Healing
  - And large scale deployment
  
 
 ### K8s Dashboard Menu
 
 - Cluster
 
 `Namespace`

Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called 	namespaces.

Namespaces are intended for use in environments with many users spread across multiple teams, or projects. For clusters with a few to tens of users, you should not need to create or think about namespaces at all. Start using namespaces when you need the features they provide.

Namespaces provide a scope for names. Names of resources need to be unique within a namespace, but not across namespaces.namespaces are a way to divide cluster resources between multiple users (via resource quota).
		
In future versions of Kubernetes, objects in the same namespace will have the same access control policies by default. It is not necessary to use multiple namespaces just to separate slightly different resources, such as different versions of the same software: use labels to distinguish resources within the same namespace.

`Nodes`

A node is a worker machine in Kubernetes, previously known as a minion. A node may be a VM or physical machine, depending on the cluster. Each node has the services necessary to run pods and is managed by the master components. The services on a node include Docker, kubelet and kube-proxy. 

Node is part of cluster and hold pod on it, each running pod and other services such kubelet is put on a node or several nodes. ACtually node is an representatif object that created by kubernetes.

`Persistent Volume`

`Roles`

`StorageClasses`

---

`Daemonsets`

`Deployment`

`Jobs`

`Pods`

`Replicaset`

`ReplicationController`

`Stateful Set`

---

`Ingress`

`Services`

---

`Config Maps`

`Persistent Volume Claims`

`Secrets`












  
