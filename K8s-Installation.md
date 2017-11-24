### Kubernetes Installation



In this section will do kubernetes installation using virtual machine, 1 VM as a master and 2 VM as a member of the cluster. you can add additional member/node by edit vagrantfile I already provide in separate folder but if you have limited resources, existing configuration will just fine.

I'm using ubuntu 64bit as master & member and set 4cpu each of them with 1GB memory since we are gonna do some test for scaling container/application. and I'm installing this on a mac laptop with 8GB memory. It is better to have baremetal server with bigger CPU and Memory allocation.

As an alternative you can use minikube as a reference and for little exploring to get known and familiar with kubernetes dashboard and kubectl command then continue on VM or directly install on baremetal server with couple of nodes. 

I also added additional network interface for Kubernetes cluster networking and a simple shell command to update hosts file during provisioning. This is required by Kubernetes networking to work when running on Vagrant because the first interface is NAT bridge and all nodes have the same IP address. After all VMs are up and running the first step is to add official Kubernetes repo and to install all required packages:

1. Vagrantfile

Let's create a folder where we can store vagrantfile to download and setup VM we needed

Make folder with sudo

`$ sudo mkdir K8s`              #create folder to put vagrantfile here

`cd to k8s and vagrant init ubuntu/xenial64` #Replace vagrantfile here with vagrantfile from file folder

`$ sudo Vagrant up`

        # Sample output from Vagrant up
        vagrant up
        Bringing machine 'master' up with 'virtualbox' provider...
        Bringing machine 'member' up with 'virtualbox' provider...
        Bringing machine 'member2' up with 'virtualbox' provider...
        ==> master: Checking if box 'ubuntu/xenial64' is up to date...
        ==> master: A newer version of the box 'ubuntu/xenial64' is available! You currently
        ==> master: have version '20171028.0.0'. The latest is version '20171121.1.0'. Run
        ==> master: `vagrant box update` to update.
        ==> master: Clearing any previously set forwarded ports...
        ==> master: Clearing any previously set network interfaces...
        ==> master: Preparing network interfaces based on configuration...
   

ssh to master VM `$ sudo vagrant ssh master`

`ubuntu@master:~$`

2. Install dependencies needed for kubernetes 

`curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -`

`echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list`

`sudo apt-get update && sudo apt-get install -y docker-engine kubelet kubeadm kubectl kubernetes-cni`

Do the same to all vm to get same dependencies required for running kubernetes cluster

3. Kubeadm

`$ sudo kubeadm init --api-advertise-addresses 192.168.5.10 --pod-network-cidr 10.144.0.0/16 --token[will show after advertise]`


        --api-advertise-addresses          #For set Ip address as a master then advertise to member
        --pod-network-cidr                 #Set pod network address range so each pod can communicate to others
        --token                            #Will generate token automaticly from master and use by member to join cluster


4. Flannel Network

we use flannel plugin network for pod to be able communicate each other, you dont need this if you run kubernetes inside GCP


#Copy k8s-flannel.yaml from file folder and run this command 


`$ kubectl create -f k8s-flannel.yaml`

wait for sometime and then run command from ubuntu master

        ubuntu@master:~$ kubectl get pods -o wide --all-namespaces
        NAMESPACE     NAME                                    READY     STATUS    RESTARTS   AGE       IP              NODE
        default       k8s-flannel-ds-b1p1z                    2/2       Running   0          1d        192.168.5.10   master
        kube-system   dummy-2088944543-nw04c                  1/1       Running   1          1d        192.168.5.10   master
        kube-system   etcd-master                             1/1       Running   1          1d        192.168.5.10   master
        kube-system   kube-apiserver-master                   1/1       Running   0          1d        192.168.5.10   master
        kube-system   kube-controller-manager-master          1/1       Running   1          1d        192.168.5.10   master
        kube-system   kube-discovery-1769846148-x68wg         1/1       Running   1          1d        192.168.5.10   master
        kube-system   kube-dns-2924299975-jf1h5               4/4       Running   0          1d        10.144.0.2     master
        kube-system   kube-proxy-hv751                        1/1       Running   1          1d        192.168.5.10   master
        kube-system   kube-scheduler-master                   1/1       Running   1          1d        192.168.5.10   master


5. Join to the cluster

we can now run kubeadm from member & member2 to join cluster
`$ kubeadm join --token=8c2350.f553************ 192.168.5.10`

Wait for a minute and see that node is in ready state:


        ubuntu@master:~$ kubectl get nodes
        NAME      STATUS         AGE
        member     Ready          1d
        member2    Ready          1d
        master     Ready,master   1d
        
        
 To start using your cluster as reguler user we need to run this command
 
`sudo cp /etc/kubernetes/admin.conf $HOME/`

`sudo chown $(id -u):$(id -g) $HOME/admin.conf`

`export KUBECONFIG=$HOME/admin.conf`

6. Add kubernetes dashboard service to our cluster

I have added K8s dashboard .yaml on file folder

`$ sudo Kubectl create -f /kubernetes-dashboard.yaml`.  #run kubectl create from master


Accessing Kubernetes dashboard or any other service from your localhost is easy. You just need to find NodePort of the service and to create forward rule on VirtualBox. Find NodePort for kubernetes-dashboard service:

Have take a look for k8s dashboard service by running kubectl describe services from master

```shell
ubuntu@master:~$ kubectl describe services kubernetes-dashboard -n kube-system
Name:                     kubernetes-dashboard
Namespace:                kube-system
Labels:                   k8s-app=kubernetes-dashboard
Annotations:              <none>
Selector:                 k8s-app=kubernetes-dashboard
Type:                     NodePort
IP:                       10.104.111.21
Port:                     <unset>  443/TCP
TargetPort:               8443/TCP
NodePort:                 <unset>  30141/TCP
Endpoints:                10.144.1.125:8443
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

Our dashboard is running on port:443 >> TargetPort:8443 >> NodePort:30141 it means we can access kube dashboard from endpoint port 30141.

But first let see where our kube master and DNS  is running 

        $ kubectl cluster-info
        Kubernetes master is running at https://192.168.5.10:6443
        KubeDNS is running at https://192.168.5.10:6443/api/v1/namespaces/kube-system/services/kube-dns/proxy

Let make sure is kube dashboard service is running so we can access dashboard

        $ kubectl get services --namespace=kube-system
        NAME                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)         AGE
        kube-dns               ClusterIP   10.96.0.10      <none>        53/UDP,53/TCP   14d
        kubernetes-dashboard   NodePort    10.104.111.21   <none>        443:30141/TCP   14d
        
Ah, it's automaticly running because I already add services with .yaml when we run kubernetes-dashboard.yaml

Run kubectl get pods to find kube dashboard 

        $ kubectl get pods -n kube-system        
        NAME                                   READY     STATUS    RESTARTS   AGE
        etcd-master                            1/1       Running   15         14d
        kube-apiserver-master                  1/1       Running   16         14d
        kube-controller-manager-master         1/1       Running   16         14d
        kube-dns-545bc4bfd4-ps55q              3/3       Running   39         14d
        k8s-flannel-ds-jq742                   1/1       Running   14         14d
        k8s-flannel-ds-sfpw5                   1/1       Running   14         14d
        k8s-flannel-ds-zxpjj                   1/1       Running   14         14d
        kube-proxy-4w2pq                       1/1       Running   13         14d
        kube-proxy-6fc5f                       1/1       Running   15         14d
        kube-proxy-bfqv7                       1/1       Running   13         14d
        kube-scheduler-master                  1/1       Running   16         14d
        kubernetes-dashboard-747c4f7cf-mdq6r   1/1       Running   11         14d
        
  Forward port from virtualbox to localhost member  NodePort:30141 >> Host Port https://<IP Address>:8443
        
  At this point we should be able to access kubernetes dashboard through 
  
  `https://192.168.15.45:8443/#!/login`
  
  <img width="719" alt="screen shot 2017-11-23 at 12 10 06 pm" src="https://user-images.githubusercontent.com/32785359/33159653-874f0f5a-d047-11e7-968c-690210e0c9ff.png">
  
 
 
 Installing kubernetes cluster with vagrant it's done, you can access kube dashboard using token, Yaml file or just can skip with admin privileges.
  
  


