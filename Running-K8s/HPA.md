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

So, now we have a hpa running for our deployment “nginx-web”. It compares the arithmetic mean of the pods’ CPU utilization with the target defined in Spec.CPUUtilization, and adjusts the replicas of the Scale if needed to match the target (preserving condition: MinReplicas <= Replicas <= MaxReplicas).

But, how you will update the minimum no. of replicas in an existing HPA? In our case, currently, we have set the minimum no. of minpods to 2, what if we need to update the min. no. of minpods to 4. In this scenerio, just we need to get the hpa in yaml format and update the yaml file. Here’s an example,

`$ kubectl get hpa/nginx-web -o yaml > nginx-hpa.yaml -n my-kube`

it will produce .yaml file 

        apiVersion: autoscaling/v1
        kind: HorizontalPodAutoscaler
        metadata:
          creationTimestamp: 2017-11-28T11:41:45Z
          name: nginx-web
          namespace: my-kube
          resourceVersion: "375119"
          selfLink: /apis/autoscaling/v1/namespaces/my-kube/horizontalpodautoscalers/nginx-web
          uid: 1c765779-d431-11e7-9e20-0251174123cb
        spec:
          maxReplicas: 5
          minReplicas: 4
          scaleTargetRef:
            apiVersion: extensions/v1beta1
            kind: Deployment
            name: nginx-web
          targetCPUUtilizationPercentage: 80
        status:
          currentReplicas: 0
          desiredReplicas: 0

`$ sudo nano nginx-hpa.yaml` # Edit minReplicas to 4

Update existing HPA nginx-web from minpods: 2 to minpods: 4 with this command

    $ kubectl apply -f nginx-hpa.yaml -n my-kube
    horizontalpodautoscaler "nginx-web" created

```shell
kubectl get hpa -n my-kube
NAME        REFERENCE              TARGETS           MINPODS   MAXPODS   REPLICAS   AGE
nginx-web   Deployment/nginx-web   <unknown> / 80%   4         5         2          1m
```





