
### Deployment

* Deploy container 


This topic is still part of running-k8s, on the previous chapter we just discuss regarding access to kube dashboard using admin privileges.

One of the basic building blocks is a pod, which is the smallest deployable unit that can be managed by Kubernetes. A pod is a logical group of one or more containers that share the same IP address and port space.

Kubernetes have few building blocks that manage a set of pods, such as a Kubernetes deployment, job, or daemon set and replication controller. we will run a pod through deployment with kubectl command or .yaml file.

Which namespace already online in our kube dashboard :

    ubuntu@master:~$ kubectl get namespace
    NAME          STATUS    AGE
    default       Active    15d
    kube-public   Active    15d
    kube-system   Active    15d
    trial         Active    14d

By default there's 3 namespace default, kube-public, kube-system. additional `namespace:trial` I've created before for learning purpose.

we'll create another one with name:`my-kube` Hope still enough resource to use since those 4 namespace already consuming my laptop resources. I'm not gonna delete old namespace right now might be usefull for me for creating this tutorial.


`kubectl create command available ;`

        Available Commands:
          clusterrole         Create a ClusterRole.
          clusterrolebinding  Create a ClusterRoleBinding for a particular ClusterRole
          configmap           Create a configmap from a local file, directory or literal value
          deployment          Create a deployment with the specified name.
          namespace           Create a namespace with the specified name
          poddisruptionbudget Create a pod disruption budget with the specified name.
          quota               Create a quota with the specified name.
          role                Create a role with single rule.
          rolebinding         Create a RoleBinding for a particular Role or ClusterRole
          secret              Create a secret using specified subcommand
          service             Create a service using specified subcommand.
          serviceaccount      Create a service account with the specified name
  
we'll use namespace below

```shell
ubuntu@master:~$ kubectl create namespace my-kube
namespace "my-kube" created
```

    ubuntu@master:~$ kubectl get namespace
    NAME          STATUS    AGE
    default       Active    15d
    kube-public   Active    15d
    kube-system   Active    15d
    my-kube       Active    2m
    trial         Active    14d
 
 `my-kube` namespace is added and we have empty and fresh virtual cluster to use.
 
Deploy nginx webserver to kubernetes cluster from asia google cloud registry :

       $ kubectl run nginx -n my-kube \
        > --image=asia.gcr.io/google_containers/nginx:latest \
        > --port=8080 \
        > --replicas=2
        deployment "nginx" created

As we see nginx is created and deployed, let's find out

        ubuntu@master:~$ kubectl get pod -n my-kube
        NAME                     READY     STATUS    RESTARTS   AGE
        nginx-67f87b9bd4-nmp4s   1/1       Running   0          5m
        nginx-67f87b9bd4-pmg66   1/1       Running   0          5m
        
        ubuntu@master:~$ kubectl get deployment -n my-kube
        NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
        nginx     2         2         2            2           5m
        
        ubuntu@master:~$ kubectl describe deployment -n my-kube
        Name:                   nginx
        Namespace:              my-kube
        CreationTimestamp:      Fri, 24 Nov 2017 07:05:18 +0000
        Labels:                 run=nginx
        Annotations:            deployment.kubernetes.io/revision=1
        Selector:               run=nginx
        Replicas:               2 desired | 2 updated | 2 total | 2 available | 0 unavailable
        StrategyType:           RollingUpdate
        MinReadySeconds:        0
        RollingUpdateStrategy:  1 max unavailable, 1 max surge
        Pod Template:
          Labels:  run=nginx
          Containers:
           nginx:
            Image:        asia.gcr.io/google_containers/nginx:latest
            Port:         8080/TCP
            Environment:  <none>
            Mounts:       <none>
          Volumes:        <none>
        Conditions:
          Type           Status  Reason
          ----           ------  ------
          Available      True    MinimumReplicasAvailable
        OldReplicaSets:  <none>
        NewReplicaSet:   nginx-67f87b9bd4 (2/2 replicas created)
        Events:
          Type    Reason             Age   From                   Message
          ----    ------             ----  ----                   -------
          Normal  ScalingReplicaSet  6m    deployment-controller  Scaled up replica set nginx-67f87b9bd4 to 2

It describe that our nginx is running with 2 pods since we set replicas=2 but they have different back name. Our deployment tells we have 2 available nginx.

Container run on my-kube namespace with labels is added, port 8080 as we set and image is pulling from asia google registry.
You can pull from gcr.io[united state] also instead of asia.gcr.io[Asia region].

Ok nginx container is running, Pods is ok, deploy status is done and replicas pods available is 2, this container still running in normal mode it won't consume high resource and we need to expose so it's accessible from outside.

Run container with 2 options, kubectl command and if you more like GUI interface you can use kube-dashboard. 

Create another container using `.yaml` file :

Please point to file folder with Deployment.yaml file 



`$ sudo mkdir nginx-deployment && cd nginx-deployment`      #Create folder to put .yaml file

`$ sudo touch Deployment.yaml && sudo nano Deployment.yaml`     #Create .yaml file and edit


```shell
apiVersion: apps/v1beta1 # for versions before 1.8.0 use apps/v1beta1
kind: Deployment
metadata:
  name: nginx-web
  namespace: my-kube
spec:
  selector:
    matchLabels:
      app: nginx-2
  replicas: 2 # tells deployment to run 2 pods matching the template
  template: # create pods using pod definition in this template
    metadata:
      # unlike pod-nginx.yaml, the name is not included in the meta data as a unique name is
      # generated from the deployment name
      labels:
        app: nginx-2
    spec:
      containers:
      - name: my-nginx
        image: asia.gcr.io/google_containers/nginx:1.7.9
        ports:
        - containerPort: 8081
```
.yaml file it's been deploy and scheduled by kube-scheduler, after few seconds run below command


    ubuntu@master:~$ kubectl get pod -n my-kube
    NAME                         READY     STATUS    RESTARTS   AGE
    nginx-67f87b9bd4-nmp4s       1/1       Running   0          1h
    nginx-67f87b9bd4-pmg66       1/1       Running   0          1h
    nginx-web-7446c487b7-fshll   1/1       Running   0          18m
    nginx-web-7446c487b7-nx45x   1/1       Running   0          18m

Second container in namespace=my-kube apply through .yaml file  are added

```shell
kubectl get deployment -n my-kube
NAME        DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
nginx       2         2         2            2           1h
nginx-web   2         2         2            2           18m
```


`ubuntu@master:~$ kubectl describe deployment nginx-web -n my-kube`  #Describe container deployment
```shell
Name:                   nginx-web
Namespace:              my-kube
CreationTimestamp:      Fri, 24 Nov 2017 08:32:09 +0000
Labels:                 app=nginx-2
Annotations:            deployment.kubernetes.io/revision=1
                        kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"apps/v1beta1","kind":"Deployment","metadata":{"annotations":{},"name":"nginx-web","namespace":"my-kube"},"spec":{"replicas":2,"selector"...
Selector:               app=nginx-2
Replicas:               2 desired | 2 updated | 2 total | 2 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx-2
  Containers:
   my-nginx:
    Image:        asia.gcr.io/google_containers/nginx:1.7.9
    Port:         8081/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   nginx-web-7446c487b7 (2/2 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  20m   deployment-controller  Scaled up replica set nginx-web-7446c487b7 to 2
```

From above description of second nginx container, we give it a name nginx-web and set to my-kube namespace, port run on 8081 and with different version of nginx from previous container.

Run kubectl command to show only second container that we just deploy using labels tag

    ubuntu@master:~$ kubectl get pod -l app=nginx-2 -n my-kube
    NAME                         READY     STATUS    RESTARTS   AGE
    nginx-web-7446c487b7-fshll   1/1       Running   0          22m
    nginx-web-7446c487b7-nx45x   1/1       Running   0          22m

Updating container using .yaml file 

`$ sudo touch Deployment_update.yaml && sudo nano Deployment_update.yaml`

```shell
apiVersion: apps/v1beta1 # for versions before 1.8.0 use apps/v1beta1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: my-kube
spec:
  selector:
    matchLabels:
      app: nginx-2
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx-2
    spec:
      containers:
      - name: nginx
        image: asia.gcr.io/google_containers/nginx:1.8 # Update the version of nginx from 1.7.9 to 1.8
        ports:
        - containerPort: 80
```        
 
    ubuntu@master:~$ kubectl apply -f nginx-Deployment/Deployment_update.yaml
    deployment "nginx-deployment" created



`Note :`

The preferred way to create a replicated application is to use a Deployment, which in turn uses a ReplicaSet but this is not horizontal pod autoscaling. Before the Deployment and ReplicaSet were added to Kubernetes, replicated applications were configured by using ReplicationController.





