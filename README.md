# os-ansible-poc

Openstack (Zed release) installation with Vagrant : 1 controller/neutron, 1 cinder, 1 or 2 nova computes, with use of openstack-ansible project.
Optionnaly, deploy on openstack a k8s (v1.27.2) on 3 to 6 vms : 1 or 3 manager(s) and 1 to 5 worker node(s).

## Requirements

Ensure you have enough system resources, at least for a small deployment
- 4 CPU Cores
- 32 Go RAM
- At least 300Go free disk space

For setup execution, you need following packages/softwares on your workstation
- An hypervisor : virtualbox or kvm (with libvirtd)
- Vagrant

## Libvirt vs Virtualbox

Need to enable virtualization (VT-x and VT-d) in your motherboard bios setting.

Default is virtualbox provider install because virtualbox is easier to use and is compliant with majority of OS.
But virtualbox dont provide performant nested virtualization and has some constraints about vlan traffic flows.

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
- Install recent (>=0.3.0) libvirt vagrant plugin
```
$ vagrant plugin install vagrant-libvirt
```

## Run

Execute ./up cmd with optional following options
```
Usage: ./up [options]
-h           this is some help text.
-d           destroy all previously openstack install
-k           deploy k8s onto 6 vms (1 master and 5 nodes) after openstack install ending
-p xxxx      vagrant provider, default is virtualbox
-s xxxx      sizing deployment, default is small
              - small : host with 32Go ram / 4 cores / 300Go free disk space
              - medium : host with 64Go ram / 6 cores / 600Go free disk space
              - large : host with 96Go ram / 8 cores / 1000Go free disk space
```

## PostInstall Instructions

Once installation is done, you need some additional setup steps into your local workstation
- Linux (replace virbr3 with your 192.168.95.1 network interface)
```
# route add -net 172.29.236.0/22 gw 192.168.95.70
# route add -net 192.168.95.0/24 dev virbr3
```
- MacOs (replace vboxnet1 with your 192.168.85.1 network interface)
```
# route add -net 172.29.236.0/22 192.168.95.70
# route add -net 192.168.90.0/24 192.168.95.70 
```

After that, you can reach the openstack dashboard
https://172.29.36.100/ (tenant0 / tenant0 and admin / toor)

## K8S Deployment

You can deploy a k8s during setup with '-k' parameter on ./up command
```
./up -k
```
Kubernetes deployment will launch multiple vms : 1 master and 2 (small) to 5 (large) nodes, and use vagrant public ssh key as openstack keypair (!! not secure).

This deployment uses this k8s install repository: https://github.com/ricofehr/yakir

Once k8s deployed, get the "loadbalancer k8s" floating ip from openstack dashboard and add this line to your /etc/hosts file
```
LOADBALANCERIP dashboard.k8s.local
```

After that you can reach the following urls
- https://dashboard.k8s.local/ => k8s dashboard
