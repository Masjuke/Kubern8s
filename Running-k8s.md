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
  
  

