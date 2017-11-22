### Picking the right solution




* Do you just want to try out Kubernetes on your computer, or do you want to build a `high-availability`, `multi-node cluster`? Choose distros best suited for your needs.

* If you are designing for `high-availability`, learn about configuring `clusters in multiple zones`.

* Will you be using a hosted Kubernetes cluster, such as `Google Kubernetes Engine`, or hosting your own cluster?

* Will your cluster be `on-premises`, or in the `cloud (IaaS)`? Kubernetes does not directly support hybrid clusters. Instead, you can set up multiple clusters.

* If you are configuring Kubernetes on-premises, consider which networking model fits best. One option for custom networking is `OpenVSwitch GRE/VxLAN networking`, which uses OpenVSwitch to set up networking between pods across Kubernetes nodes.

* Will you be running Kubernetes on `bare metal` hardware or on `virtual machines (VMs)`?

* Do you just want to run a cluster, or do you expect to do active development of Kubernetes project code? If the latter, choose a actively-developed distro. Some distros only use binary releases, but offer a greater variety of choices.

* Familiarize yourself with the components needed to run a cluster.
