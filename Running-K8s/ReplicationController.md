### Replication Controller

Summary 

`A ReplicationController ensures that a specified number of pod replicas are running at any one time. In other words, a ReplicationController makes sure that a pod or a homogeneous set of pods is always up and available.`

`If there are too many pods, the ReplicationController terminates the extra pods. If there are too few, the ReplicationController starts more pods. Unlike manually created pods, the pods maintained by a ReplicationController are automatically replaced if they fail, are deleted, or are terminated. For example, your pods are re-created on a node after disruptive maintenance such as a kernel upgrade. For this reason, you should use a ReplicationController even if your application requires only a single pod. A ReplicationController is similar to a process supervisor, but instead of supervising individual processes on a single node, the ReplicationController supervises multiple pods across multiple nodes.`

`ReplicationController is often abbreviated to “rc” or “rcs” in discussion, and as a shortcut in kubectl commands.
A simple case is to create one ReplicationController object to reliably run one instance of a Pod indefinitely. A more complex use case is to run several identical replicas of a replicated service, such as web servers.`


Sample of replication controller runs of 4 copies nginx webserver

`$ sudo touch Replication-Controller.yaml && sudo nano Replication-Controller.yaml`

    apiVersion: apps/v1beta1
    kind: ReplicationController
    metadata:
      name: nginx-web
    spec:
      replicas: 4
      selector:
        app: nginx
      template:
        metadata:
          name: nginx-2
          labels:
            app: nginx-2
        spec:
          containers:
          - name: my-nginx
            image: nginx:latest
            ports:
            - containerPort: 80
            
  
  `$ kubectl apply -f ./Replication-Controller.yaml -n my-kube` #Create nginx webserver replication controller
  
  Now check status our RC using below command
  
  `$ kubectl describe rc/nginx-web -n my-kube` #Check status of RC nginx-web
  
```shell  
Name:         nginx-web
Namespace:    my-kube
Selector:     app=nginx
Labels:       app=nginx
Annotations:  kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","kind":"ReplicationController","metadata":{"annotations":{},"name":"nginx-web","namespace":"my-kube"},"spec":{"replicas":4,"selector...
Replicas:     4 current / 4 desired
Pods Status:  4 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=nginx
  Containers:
   my-nginx:
    Image:        nginx:latest
    Port:         80/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Events:
  Type    Reason            Age   From                    Message
  ----    ------            ----  ----                    -------
  Normal  SuccessfulCreate  2m    replication-controller  Created pod: nginx-web-gxhsp
  Normal  SuccessfulCreate  2m    replication-controller  Created pod: nginx-web-kcg74
  Normal  SuccessfulCreate  2m    replication-controller  Created pod: nginx-web-xxhrh
  Normal  SuccessfulCreate  2m    replication-controller  Created pod: nginx-web-kndj2
  ```
  
 You can also check status using kubernetes-dashboard UI
 
 <img width="1433" alt="screen shot 2017-11-27 at 3 07 17 pm" src="https://user-images.githubusercontent.com/32785359/33256505-e4b36700-d384-11e7-81ff-aedba26d6d20.png">
 
As we can see nginx-web copies is added.
You can specify how many pods should run concurrently by setting .spec.replicas to the number of pods you would like to have running concurrently. The number running at any time may be higher or lower, such as if the replicas were just increased or decreased, or if a pod is gracefully shutdown, and a replacement starts early.
If you do not specify .spec.replicas, then it defaults to 1.



