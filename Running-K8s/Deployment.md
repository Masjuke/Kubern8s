
### Deployment

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

`$ kubectl create namespace my-kube`

    ubuntu@master:~$ kubectl get namespace
    NAME          STATUS    AGE
    default       Active    15d
    kube-public   Active    15d
    kube-system   Active    15d
    my-kube       Active    2m
    trial         Active    14d
 
 `my-kube` namespace is added now and ready to use
 
 
```shell
ubuntu@master:~$ kubectl create namespace my-kube
namespace "my-kube" created
```

`$ kubectl run nginx --image=gcr.io/google_containers/nginx:latest`    #Pull nginx from GCR and deploy to kubernetes




