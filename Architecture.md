
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

Persistent volume is a piece of storage volume that used cluster resources same as node used by pod and consume cluster resource, persistent volume `PV` is assign for user by administrator to a specific pod to store data. we can also uses external storage volume instead using from kubernetes  volume especially for storing database.

`Roles`

Sets of description rule inside kubernetes for granting user access, easy way is a collection of permission rules for example permission on pod or permission for pod. this is defined in RBAC using 2 objects between user and resources

`StorageClasses`

It's a dynamic provisioning volume for containers/pod based on demand for critical path of running stateful container



---


`Daemonsets`

Daemonsets of kubernetes is a running service inside of kubernetes system for replicas a pod to run on specific node or on all nodes. it can create shared storage and logging for all pod also deploy monitoring agent on all nodes.

You can manage replicas and scale pod through daemonset besides HPA or RC.

`Deployment`

Deploying application or container into node or pods, to do this it need API version kind:Deployment. there's few ways strategies to deploy application/container 

`Jobs`

Job on kubernetes is 

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












  
