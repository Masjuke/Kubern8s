### Additional notes from me


1. Create yaml file  with kind:replication controller will not show in deployment or pod, and also will not show automatic in service and autoscale and horizontal pod autoscale 

2. Create yaml file with kind:deployment will show in replicas and pod but will not show in service

3. Create yaml file with kind: pod will now show in deployment and replicas and service

4. service need to expose within yams file when creation or manually

5. Kube-proxy is set if we need access kubernetes-dashboard through http

6. CNI is default network or compatible network for docker container in kubernetes

7. It is better arrange cluster kubernetes from the beginning foe ease configuration in the next phase

8. We can create separate namespace for every user such admin, developer, SRE etc, with different access like token, certificate. except admin access can see all namespace

9. Kind:deployment will automatically set replicas as 1, if pod is deleted it will auto healing

10. same like kind:replicatilon controller it will set auto healing if pod is deleted

11. kind:pod is a naked pod this is not recommended because if pod is erase it will delete permanently

12. Horizontal pod autoscaler need to set within yams file or manually from kubectl

13. Only deployment and replication controller the a pod can be set to horizontal pod autoscale. Pod cannot directly set to horizontal autoscaler.

14. Ingress purpose is to set service can be access outside world

15. If we are using GKE as kubernetes platform we don’t need to install network plugin

16. Network plugin is needed when we install kubernetes not in cloud GKE

17. Load balancing container service only available in cloud provider

18. Best way to deploy app is through replication controller

19. Deployments are a newer and higher level concept than Replication Controllers. 

```shell
They manage the deployment of Replica Sets (also a newer concept, but pretty much equivalent to Replication Controllers), 
and allow for easy updating of a Replica Set as well as the ability to roll back to a previous deployment, 
Previously this would have to be done with kubectl rolling-update which was not declarative and did not provide the rollback features.
```

20. Flannel is the most suitable network plugin for kubernetes at this time

21. Use standalone volume instead datastore kubernetes

22. Use HA proxies backend to connect to pod, but ingress also fine
