GNS3
=====

This directory contains a *Vagrantfile* and *vagrant-gns3.yml* to build
an environment to run [gns3](https://www.gns3.com/).

You can get images at the
[gns3 marketplace](https://www.gns3.com/marketplace/appliances).

Make sure your guest has sufficient memory and disk space for the gns3
appliances. 

Nested Virtualization
--------------------------

You will need to enable nested virtualization to effectively run *gns3*.

* Make sure you load kvm_intel with *nested=yes*
```
# modprobe -r kvm
# modprobe kvm_intel nested=yes
```
* Edit */etc/modprobe.d/kvm.conf* with:
```
options kvm_intel nested=1
```
* Expose virt extensions to the guest hypervisor
```
# virsh edit guestname

  <cpu mode='host-model'>
    <model fallback='allow'/>
    <feature policy='require' name='vmx'/>
    <feature policy='require' name='smx'/>
  </cpu>
```
