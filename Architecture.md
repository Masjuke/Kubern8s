
### Understanding of K8s Dashboard-Component 

### Cluster

  K8s cluster is made of master node and worker nodes, Collection of several Physical or Virtual machine. 1 set as master node of cluster. 
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
 
 * Namespace


`Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called 	namespaces.`

`Namespaces are intended for use in environments with many users spread across multiple teams, or projects. For clusters with a few to tens of users, you should not need to create or think about namespaces at all. Start using namespaces when you need the features they provide.`

`Namespaces provide a scope for names. Names of resources need to be unique within a namespace, but not across namespaces.namespaces are a way to divide cluster resources between multiple users (via resource quota).`
		
`In future versions of Kubernetes, objects in the same namespace will have the same access control policies by default. It is not necessary to use multiple namespaces just to separate slightly different resources, such as different versions of the same software: use labels to distinguish resources within the same namespace.`


* Nodes

`A node is a worker machine in Kubernetes, previously known as a minion. A node may be a VM or physical machine, depending on the cluster. Each node has the services necessary to run pods and is managed by the master components. The services on a node include Docker, kubelet and kube-proxy.` 

`Node is part of cluster and hold pod on it, each running pod and other services such kubelet is put on a node or several nodes. ACtually node is an representatif object that created by kubernetes.`


* Persistent Volume


`Persistent volume is a piece of storage volume that used cluster resources same as node used by pod and consume cluster resource, persistent volume `PV` is assign for user by administrator to a specific pod to store data. we can also uses external storage volume instead using from kubernetes  volume especially for storing database.`


* Roles

`Sets of description rule inside kubernetes for granting user access, easy way is a collection of permission rules for example permission on pod or permission for pod. this is defined in RBAC using 2 objects between user and resources.`


* StorageClasses

`It's a dynamic provisioning volume for containers/pod based on demand for critical path of running stateful container.`


---

* Daemonsets

`Daemonsets of kubernetes is a running service inside of kubernetes system for replicas a pod to run on specific node or on all nodes. it can create shared storage and logging for all pod also deploy monitoring agent on all nodes. You can manage replicas and scale pod through daemonset besides HPA or RC.`


* Deployment

`Deploying application or container into node or pods, to do this it need API version kind:Deployment. there's few ways strategies to deploy application/container.`


* Jobs

`Jobs on kubernetes is a supervisor for pods carrying out batch process, process that run on a certain time of completion
and given scheduled. this can be done through cron job like crontab.`


* Pods

`Kubernetes pods is a group or collection of container/application on node cluster that can be deploy together or single pod, basicly pods is building block of k8s the smallest and simplest kubernetes object that we create of deploy.`


* Replica Sets

`Replica sets is created automaticly when we deploy container through kind:deployment the purpose is as a backend of depoyment, `RS` function is to manage all pods using labels that match with the selector, it does not distinguish between pods created or deleted and pods that another person or process created or deleted.`


* ReplicationController

`Replication Controller is a structure that enables you to easily create multiple pods, then make sure that that number of pods always exists. If a pod does crash, the Replication Controller replaces it. Replication Controllers also provide other benefits, such as the ability to scale the number of pods, and to update or delete multiple pods with a single command. You can create a Replication Controller with an imperative command, or declaratively, from a file.`


* Stateful Set

`To manage the deployment and scaling a sets of pods. A Pod represents a set of running containers on your cluster., and provides guarantees about the ordering and uniqueness of these Pods.`


---

* Ingress

`Ingress controller is to manage services of application/containers to be accessible from external, ingress controller is deploy on master act as daemon and deploy on kubernetes as a pod to watch APISERVER/ingress endpoint for update to ingress resources.`

* Services

`After we deployed containers/application through deployment/Replicasets/ReplicationController, the container need to be accessible and therefore run `kubectl expose` to manage services of a container inside pods and set `port` number and type of port.`

`A Kubernetes Service is an abstraction which defines a logical set of Pods and a policy by which to access them - sometimes called a micro-service. The set of Pods targeted by a Service is (usually) determined by a Label Selector (see below for why you might want a Service without a selector).`


---


* Config Maps

`It's a place to store configuration of `ConfigMap` data as a key value store. for example when you created volume through configmap, each data it will represent as an individual file volume.`


* Persistent Volume Claims

`PVC is similar to pod and it's main purpose is to provide storage requested by user.`


* Secrets

`Every user that have access to kubernetes can have their own secret with different privileges and path, for example dev team and admin have own level access of secret to manage container/application on kubernetes.`

`Convert your secret data to a base-64 representation. Create a Secret. Create a Pod that has access to the secret data through a Volume. Create a Pod that has access to the secret data through environment variables.`










  
