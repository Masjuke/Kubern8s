### Daemon Sets


A DaemonSet ensures that all or some Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.

Some typical uses of a DaemonSet are:

* running a cluster storage daemon, such as glusterd, ceph, on each node.

* running a logs collection daemon on every node, such as fluentd or logstash.

* running a node monitoring daemon on every node, such as Prometheus Node Exporter, collectd, Datadog agent, New Relic agent, or Ganglia gmond.

In a simple case, one DaemonSet, covering all nodes, would be used for each type of daemon. A more complex setup might use multiple DaemonSets for a single type of daemon, but with different flags and/or different memory and cpu requests for different hardware types.

`If node labels are changed, the DaemonSet will promptly add Pods to newly matching nodes and delete Pods from newly not-matching nodes.`


### Daemon Sets Alternatives

`Bare Pods`

It is possible to create Pods directly which specify a particular node to run on. However, a DaemonSet replaces Pods that are deleted or terminated for any reason, such as in the case of node failure or disruptive node maintenance, such as a kernel upgrade. For this reason, you should use a DaemonSet rather than creating individual Pods.


`Static Pods`

It is possible to create Pods by writing a file to a certain directory watched by Kubelet. These are called static pods. Unlike DaemonSet, static Pods cannot be managed with kubectl or other Kubernetes API clients. Static Pods do not depend on the apiserver, making them useful in cluster bootstrapping cases. Also, static Pods may be deprecated in the future.


`Deployments`

DaemonSets are similar to Deployments in that they both create Pods, and those Pods have processes which are not expected to terminate (e.g. web servers, storage servers).

Use a Deployment for stateless services, like frontends, where scaling up and down the number of replicas and rolling out updates are more important than controlling exactly which host the Pod runs on. Use a DaemonSet when it is important that a copy of a Pod always run on all or certain hosts, and when it needs to start before other Pods.

### Sample of Daemon Sets

Actually we did already run daemonsets for network plugin for arranging pods communincation since we are install kubernetes on VM. Have a take a look :


    $ kubectl get daemonsets -n kube-system
    NAME              DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                   AGE
    k8s-flannel-ds    3         3         3         3            3           beta.kubernetes.io/arch=amd64   19d
    kube-proxy        3         3         3         3            3           <none>                          19d

There's two daemon sets running on kube-system namespace.


`$ kubectl get daemonset kube-flannel-ds -n kube-system -o=yaml` #Get .yaml file from existing daemon set

```shell
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"extensions/v1beta1","kind":"DaemonSet","metadata":{"annotations":{},"labels":{"app":"flannel","tier":"node"},"name":"k8s-flannel-ds","namespace":"kube-system"},"spec":{"template":{"metadata":{"labels":{"app":"flannel","tier":"node"}},"spec":{"containers":[{"command":["/opt/bin/flanneld","--ip-masq","--kube-subnet-mgr"],"env":[{"name":"POD_NAME","valueFrom":{"fieldRef":{"fieldPath":"metadata.name"}}},{"name":"POD_NAMESPACE","valueFrom":{"fieldRef":{"fieldPath":"metadata.namespace"}}}],"image":"quay.io/coreos/flannel:v0.9.0-amd64","name":"k8s-flannel","securityContext":{"privileged":true},"volumeMounts":[{"mountPath":"/run","name":"run"},{"mountPath":"/etc/k8s-flannel/","name":"flannel-cfg"}]}],"hostNetwork":true,"initContainers":[{"args":["-f","/etc/k8s-flannel/cni-conf.json","/etc/cni/net.d/10-flannel.conf"],"command":["cp"],"image":"quay.io/coreos/flannel:v0.9.0-amd64","name":"install-cni","volumeMounts":[{"mountPath":"/etc/cni/net.d","name":"cni"},{"mountPath":"/etc/k8s-flannel/","name":"flannel-cfg"}]}],"nodeSelector":{"beta.kubernetes.io/arch":"amd64"},"serviceAccountName":"flannel","tolerations":[{"effect":"NoSchedule","key":"node-role.kubernetes.io/master","operator":"Exists"}],"volumes":[{"hostPath":{"path":"/run"},"name":"run"},{"hostPath":{"path":"/etc/cni/net.d"},"name":"cni"},{"configMap":{"name":"k8s-flannel-cfg"},"name":"flannel-cfg"}]}}}}
  creationTimestamp: 2017-11-08T07:41:32Z
  generation: 1
  labels:
    app: flannel
    tier: node
  name: k8s-flannel-ds
  namespace: kube-system
  resourceVersion: "344008"
  selfLink: /apis/extensions/v1beta1/namespaces/kube-system/daemonsets/k8s-flannel-ds
  uid: 3d783fb5-c458-11e7-a50f-0251174123cb
spec:
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: flannel
      tier: node
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: flannel
        tier: node
    spec:
      containers:
      - command:
        - /opt/bin/flanneld
        - --ip-masq
        - --kube-subnet-mgr
        env:
        - name: POD_NAME
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: metadata.name
        - name: POD_NAMESPACE
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
        image: quay.io/coreos/flannel:v0.9.0-amd64
        imagePullPolicy: IfNotPresent
        name: k8s-flannel
        resources: {}
        securityContext:
          privileged: true
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        volumeMounts:
        - mountPath: /run
          name: run
        - mountPath: /etc/k8s-flannel/
          name: flannel-cfg
      dnsPolicy: ClusterFirst
      hostNetwork: true
      initContainers:
      - args:
        - -f
        - /etc/k8s-flannel/cni-conf.json
        - /etc/cni/net.d/10-flannel.conf
        command:
        - cp
        image: quay.io/coreos/flannel:v0.9.0-amd64
        imagePullPolicy: IfNotPresent
        name: install-cni
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        *****
```

Logs DaemonSets from kube-dashboard :

```shell
I1128 03:40:08.516366       1 main.go:470] Determining IP address of default interface
I1128 03:40:08.518166       1 main.go:483] Using interface with name enp0s3 and address 10.0.2.15
I1128 03:40:08.518188       1 main.go:500] Defaulting external address to interface address (10.0.2.15)
I1128 03:40:08.539037       1 kube.go:130] Waiting 10m0s for node controller to sync
I1128 03:40:08.539086       1 kube.go:283] Starting kube subnet manager
I1128 03:40:09.543122       1 kube.go:137] Node controller sync successful
I1128 03:40:09.543151       1 main.go:235] Created subnet manager: Kubernetes Subnet Manager - master
I1128 03:40:09.543156       1 main.go:238] Installing signal handlers
I1128 03:40:09.543475       1 main.go:348] Found network config - Backend type: vxlan
I1128 03:40:09.543629       1 vxlan.go:119] VXLAN config: VNI=1 Port=0 GBP=false DirectRouting=false
I1128 03:40:09.594185       1 main.go:295] Wrote subnet file to /run/flannel/subnet.env
I1128 03:40:09.594199       1 main.go:299] Running backend.
I1128 03:40:09.594207       1 main.go:317] Waiting for all goroutines to exit
I1128 03:40:09.594230       1 vxlan_network.go:56] watching for new subnet leases
I1128 03:40:09.614303       1 ipmasq.go:75] Some iptables rules are missing; deleting and recreating rules
I1128 03:40:09.614339       1 ipmasq.go:97] Deleting iptables rule: -s 10.144.0.0/16 -d 10.144.0.0/16 -j RETURN
I1128 03:40:09.616201       1 ipmasq.go:97] Deleting iptables rule: -s 10.144.0.0/16 ! -d 224.0.0.0/4 -j MASQUERADE
I1128 03:40:09.617466       1 ipmasq.go:97] Deleting iptables rule: ! -s 10.144.0.0/16 -d 10.144.0.0/24 -j RETURN
I1128 03:40:09.619469       1 ipmasq.go:97] Deleting iptables rule: ! -s 10.144.0.0/16 -d 10.144.0.0/16 -j MASQUERADE
I1128 03:40:09.621624       1 ipmasq.go:85] Adding iptables rule: -s 10.144.0.0/16 -d 10.144.0.0/16 -j RETURN
I1128 03:40:09.624133       1 ipmasq.go:85] Adding iptables rule: -s 10.144.0.0/16 ! -d 224.0.0.0/4 -j MASQUERADE
I1128 03:40:09.626660       1 ipmasq.go:85] Adding iptables rule: ! -s 10.144.0.0/16 -d 10.144.0.0/24 -j RETURN
I1128 03:40:09.636191       1 ipmasq.go:85] Adding iptables rule: ! -s 10.144.0.0/16 -d 10.144.0.0/16 -j MASQUERADE
```


