### Kubernetes Installation



In this section will do kubernetes installation using virtual machine, 1 VM as a master and 2 VM as a member of the cluster. you can add additional member/node by edit vagrantfile I already provide in separate folder but if you have limited resources, existing configuration will just fine.

I'm using ubuntu 64bit as master & member and set 4cpu each of them with 1GB memory since we are gonna do some test for scaling container/application. and I'm installing this on a mac laptop with 8GB memory. It it better to have baremetal server with bigger CPU and Memory allocation.

As an alternative you can use minikube as a reference and for little exploring to get known and familiar with kubernetes dashboard and kubectl command then continue on VM or directly install on baremetal server with couple of nodes. 

I also added additional network interface for Kubernetes cluster networking and a simple shell command to update hosts file during provisioning. This is required by Kubernetes networking to work when running on Vagrant because the first interface is NAT bridge and all nodes have the same IP address. After all VMs are up and running the first step is to add official Kubernetes repo and to install all required packages:

1. Vagrantfile

Let's create a folder where we can store vagrantfile to download and setup VM we needed

Make folder with sudo

`$ sudo mkdir K8s`              #You can set different folder name

`cd to k8s and vagrant init ubuntu/xenial64`

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

`$ sudo kubeadm init --api-advertise-addresses 192.168.5.10 --pod-network-cidr 10.244.0.0/16 --token[will show after advertise]`

Above command to advertise master IP address as master of the cluster and set POD network that we add using flannel





