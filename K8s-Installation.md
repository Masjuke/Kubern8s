### Kubernetes Installation

In this section will do kubernetes installation using virtual machine, 1 VM as a master and 2 VM as a member of the cluster. you can add additional member/node by edit vagrantfile I already provide in separate folder but if you have limited resources, existing configuration will just fine.

I'm using ubuntu 64bit for master & member and set 4cpu each of them with 1GB memory since we are gonna do some test for scaling container/application. and I'm installing this on a mac laptop with 8GB memory. It it better to have baremetal server with bigger CPU and Memory allocation.

As an alternative you can use minikube as a reference and for little exploring to get known and familiar with kubernetes dashboard and kubectl command then continue on VM or directly install on baremetal server with couple of nodes. 


