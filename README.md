# os-ansible-poc

Openstack installation with Vagrant : 1 controller/neutron, 1 cinder, 2 computes, with use of openstack-ansible project.
Optionnaly, deploy into it a k8s on 3 vms : 1 master and 2 nodes.

## Requirements

Ensure you have enough system resources
- 6 CPU Cores / 12Ht
- 90 Go RAM
- 1To Disk

You can make change in Vagrantfile if this doesnt fit with your system.

For setup execution, you need following packages/softwares on your workstation
- An hypervisor : virtualbox or kvm (recommanded) with libvirtd
- Vagrant

## Libvirt vs Virtualbox

Default is virtualbox provider install because virtualbox is easier to use and is compliant with majority of OS. But virtualbox dont provide well nested virtualization.

If you are onto Linux, you should use libvirt provider instead, better performance and full nested virtualization
- Install libvirtd daemon
- Ensure kvm module is installed into the kernel
- Ensure nested virtualization is enabled with kvm module
```
$ cat /sys/module/kvm_intel/parameters/nested
Y
$ cat /etc/modprobe.d/kvm.conf
options kvm_intel nested=1
```
- Install libvirt vagrant plugin: vagrant install
```
$ vagrant plugin install vagrant-libvirt
```

## Run

Execute ./up cmd with optional following options
```
Usage: ./up [options]
-h           this is some help text.
-d           destroy all previously openstack install
-p xxxx      vagrant provider, default is virtualbox
-k           deploy k8s onto 3 vms (1 master and 2 nodes) after openstack install
```

## PostInstall Instructions

Once installation is done, you need some additional setup steps into your local workstation (replace virbr3 with your 192.168.85.1 network interface)
```
route add -net 172.29.236.0/22 gw 192.168.80.70
route add -net 192.168.90.0/24 dev virbr3
```

After that, you can reach the openstack dashboard
https://172.29.36.100/ (tenant0 / tenant0 and admin / toor)

## K8S Deployment

You can deploy a k8s during setup with '-k' parameter on ./up command
```
./up -k
```
Kubernetes deployment will launch 3 vms : 1 master and 2 nodes, and use vagrant public ssh key as openstack keypair.

This deployment uses this k8s install repository: https://github.com/ricofehr/k8s

Once k8s deployed, get the "loadbalancer k8s" floating ip from openstack dashboard and add this line to your /etc/hosts file
```
LOADBALANCERIP gogs.k8s.local sonar.k8s.local keycloak.k8s.local registry.k8s.local concourse.k8s.local dashboard.k8s.local
```

After that you can reach the following urls
- https://dashboard.k8s.local/ => k8s dashboard
- https://gogs.k8s.local/ => gogs git repository
- https://sonar.k8s.local/ => Quality code scan tool
- https://registry.k8s.local/ => private docker registry
- https://concourse.k8s.local/ => concourse CI/CD tool
- https://keycloak.k8s.local/ (keycloak / keypass) => RedHat SSO tool
