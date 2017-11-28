### Horizontal Pod Autoscaler

Horizontal Pod Autoscaling automatically scales the number of pods in a replication controller, deployment or replica set based on observed CPU utilization.

Ok now we will scale with HPA existing deployment that we already created, execute `kubectl get deployment` to finds out our current deployment application/container.

```shell
$ Kubectl get deployment --namespace=my-kube
NAME        DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
nginx       2         2         2            2           4d
nginx-web   2         2         2            2           4d
```

we will scale nginx-web container with below command

    $ kubectl autoscale deployment nginx-web  --min=2 \ 
    > --max=5 --cpu-percent=80 -n my-kube

`deployment "nginx-web" autoscaled`

    $ kubectl get hpa -n my-kube
    NAME        REFERENCE              TARGETS           MINPODS   MAXPODS   REPLICAS   AGE
    nginx-web   Deployment/nginx-web   <unknown> / 80%   2         5         0          59s

