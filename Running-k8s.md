### Run kubernetes Dashboard 

* User access

`All Kubernetes clusters have two categories of users: service accounts managed by Kubernetes, and normal users.`

`Normal users are assumed to be managed by an outside, independent service. An admin distributing private keys, a user store like Keystone or Google Accounts, even a file with a list of usernames and passwords. In this regard, Kubernetes does not have objects which represent normal user accounts. Regular users cannot be added to a cluster through an API call.`

`In contrast, service accounts are users managed by the Kubernetes API. They are bound to specific namespaces, and created automatically by the API server or manually through API calls. Service accounts are tied to a set of credentials stored as Secrets, which are mounted into pods allowing in cluster processes to talk to the Kubernetes API.`

`API requests are tied to either a normal user or a service account, or are treated as anonymous requests. This means every process inside or outside the cluster, from a human user typing kubectl on a workstation, to kubelets on nodes, to members of the control plane, must authenticate when making requests to the API server, or be treated as an anonymous user.`

`Probably some of your users need to access to kubernetes like developer team, devops team Etc, and each of them need separate privileges and accessible which namespace, pods and deployment they allow to manage then creating different access token or yaml file is needed in this case.`

`If admin is the only one who can access and have all privileges kubernetes configuration and manage all the service and still want to add another users to have read access that will just fine also.`


In this section we will use admin account instead adding another user, set different namespace Etc. 


`$ kubectl config set-credentials cluster-admin --token=bearer_token`    #Add user admin for accessing kubernetes-dashboard 

    $ cat <<EOF | kubectl create -f -
    apiVersion: rbac.authorization.k8s.io/v1beta1
    kind: ClusterRoleBinding
    metadata:
      name: kubernetes-dashboard
      labels:
        k8s-app: kubernetes-dashboard
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: cluster-admin
    subjects:
    - kind: ServiceAccount
      name: kubernetes-dashboard
      namespace: kube-system
    EOF

I have already provided .yaml file for accessing kubernetes-dashboard on file folder with name `kube-admin.yaml`

 <img width="719" alt="screen shot 2017-11-23 at 12 10 06 pm" src="https://user-images.githubusercontent.com/32785359/33159653-874f0f5a-d047-11e7-968c-690210e0c9ff.png">
  
 Since we already added admin access we do not need to add token or .yaml file manually to this kube dashboard page login so we can push skip button
 
 <img width="1243" alt="screen shot 2017-11-23 at 4 47 48 pm" src="https://user-images.githubusercontent.com/32785359/33166792-20b08fc2-d06e-11e7-9176-4769ed745ef1.png">

Admin user can access kube dashboard now then we need to go next step.

https://github.com/kubernetes/dashboard/wiki/Access-control


Besides above method we can also check admin token by issue kubectl command 

```shell
kubectl get secret -n kube-system
NAME                                     TYPE                                  DATA      AGE
attachdetach-controller-token-dtmr9      kubernetes.io/service-account-token   3         15d
bootstrap-signer-token-qvhhc             kubernetes.io/service-account-token   3         15d
certificate-controller-token-wmvkp       kubernetes.io/service-account-token   3         15d
cronjob-controller-token-c7k78           kubernetes.io/service-account-token   3         15d
daemon-set-controller-token-hvrzd        kubernetes.io/service-account-token   3         15d
default-token-wnxn6                      kubernetes.io/service-account-token   3         15d
deployment-controller-token-drkxh        kubernetes.io/service-account-token   3         15d
disruption-controller-token-9zg5z        kubernetes.io/service-account-token   3         15d
endpoint-controller-token-7rg8n          kubernetes.io/service-account-token   3         15d
flannel-token-rmwbk                      kubernetes.io/service-account-token   3         15d
generic-garbage-collector-token-fq5kx    kubernetes.io/service-account-token   3         15d
heapster-token-hmjhc                     kubernetes.io/service-account-token   3         9d
horizontal-pod-autoscaler-token-m5scm    kubernetes.io/service-account-token   3         15d
job-controller-token-x2v2l               kubernetes.io/service-account-token   3         15d
juke-token-9czw6                         kubernetes.io/service-account-token   3         14d
kube-dns-token-6jm4h                     kubernetes.io/service-account-token   3         15d
kube-proxy-token-6kbfk                   kubernetes.io/service-account-token   3         15d
kubernetes-dashboard-certs               Opaque                                2         14d
kubernetes-dashboard-key-holder          Opaque                                2         14d

#--kubernetes-dashboard-token-qtbct         kubernetes.io/service-account-token   3         14d--#

namespace-controller-token-6tjj4         kubernetes.io/service-account-token   3         15d
node-controller-token-l8sp4              kubernetes.io/service-account-token   3         15d
persistent-volume-binder-token-rxdqv     kubernetes.io/service-account-token   3         15d
pod-garbage-collector-token-w2wtv        kubernetes.io/service-account-token   3         15d
replicaset-controller-token-5ztk6        kubernetes.io/service-account-token   3         15d
replication-controller-token-p9n6r       kubernetes.io/service-account-token   3         15d
resourcequota-controller-token-997ws     kubernetes.io/service-account-token   3         15d
service-account-controller-token-68z9p   kubernetes.io/service-account-token   3         15d
service-controller-token-mh29d           kubernetes.io/service-account-token   3         15d
statefulset-controller-token-4frb6       kubernetes.io/service-account-token   3         15d
token-cleaner-token-pbb57                kubernetes.io/service-account-token   3         15d
ttl-controller-token-cmmwh               kubernetes.io/service-account-token   3         15d
```





```shell
kubectl describe secret kubernetes-dashboard-token-qtbct -n kube-system
Name:         kubernetes-dashboard-token-qtbct
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=kubernetes-dashboard
              kubernetes.io/service-account.uid=cdc54b97-c476-11e7-a50f-0251174123cb

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJrdWJlcm**********************************************
```



Using token `kubernetes-dashboard-token-qtbct` to have full access admin privileges by copying 



`token:eyJhbGciOiJSUzI1*********.`      #Copy to kubernetes-dashboard login page



---

* Container Deployment





