vagrant-builder
===============

This is intended to set up a workstation for vagrant and build base
boxes for use with Vagrant and Libvirt.  The playbook
**vagrant-setup.yml** is tested with Fedora 21, but should also work
with Fedora 22 and later.

There are two ansible playbooks:

- **vagrant-setup.yml**:  Installs the necessary packages and does other
   actions that requires root to prepare the workstation.
- **vagrant-builder.yml**: Copies configuration files to *~/builder* and
   runs *virt-builder* to create boxes, as defined in inventory.

General usage:

    ansible-playbook vagrant-setup.yml
    ansible-playbook vagrant-builder.yml

Customization
-------------

If you want to customize the build of your *vagrant* *box*, you can
customize the inventory variables in *group_vars/all*.  Variables are
described below.

Inventory variables can be overridden by editing the group inventory file or
updating a *host_vars/localhost*.  The latter method is recommended.

Varible definitions
-------------------

Unless the variable is defined above, be cautious in changing these
variables.

- **vagrant_pkgs**: List of packages to install to use vagrant.
  Defaults to `vagrant`, `vagrant-libvirt`, `libvirt`,
  `libvirt-daemon-kvm`, `libguestfs-tools`, `libguestfs-xfs`, and
  `virt-install`.
- **user_home**: Value of `$HOME`.
- **builder_dir**: Where to build the boxes.  Defaults to
  `$HOME/builder`.
- **format**: Format for image.  Defaults to `qcow2`.
- **size**: Size of image.  Defaults to `10`G.
- **vagrant_metadata**:  A JSON string used for *metadata.json* that
  defines format, size, and provider.
- **password**: Password used for users `root` and `vagrant`.  Defaults
  to `vagrant`.
- **virt_builder_install**:  List of packages to install, regardless of
  distribution.  Defaults to `rsync`, `sudo`, `acpid`, `openssh-server`,
  `openssh-clients`, and `wget`.  Can be overrode on a per box bases.
- **virt_builder_options**:  Options passed to *virt-builder* when
  creating the image.  These are options common to any image creation.
- **virt_builder_images**: List of images to build for vagrant.  Each
  element in the list is a dictionary with two keys: `name` (the guest
  to build), `install` (list of packages to install), and `options` (the
  options to pass to *virt-builder* when building a specific image).
  `install` will override the default list of packages to install.
  `options` works in conjunction with `virt_builder_options`.

Workflow
--------

The following is an example workflow:

1. Set up your workstation for *vagrant*.  Only needs to be run one.
```
$ ansible-playbook vagrant-setup.yml
```
2. Build *vagrant* boxes for development and testing purposes.  You may
want to consider updating the *virt_builder_images* variable.  This can
be run as needed to build new boxes or update existing ones.
```
$ $EDITOR host_vars/localhost
$ ansible-playbook vagrant-builder.yml
```
3. Change to a working directory (eg. `cd provision`) and run
*vagrant*.  You can copy the *Vagrantfile* to the subdirectory.  You
probably also want to create an Ansible playbook to provision your
Vagrant machine.
```
$ cp Vagrantfile provision/ && cd provision
$ $EDITOR Vagrantfile
$ $EDITOR vagrant.yml
$ vagrant up
$ vagrant provision
```

You can test the image by importing it directly into libvirt using
*virt-install --import* option:

```
# virt-install --import --name guest --ram 2048 \
   --disk path=box.img,format=raw --os-variant fedora23
```

You can get a list of OS variants with `osinfo-query os`.

Possible Issues
-------------------

* Error message from Vagrant that says:  *Call to virStorageVolGetInfo
  failed: Storage volume not found*.  The trick is to refresh the
  libvirt storage pools.  Example:
```
$  sudo virsh pool-list
 Name                 State      Autostart 
-------------------------------------------
 default              active     yes       
 tmp                  active     yes 
$  sudo virsh pool-refresh tmp
Pool tmp refreshed
$  sudo virsh pool-refresh default
Pool default refreshed
```
  See https://kushaldas.in/posts/storage-volume-error-in-libvirt-with-vagrant.html

Miscellaneous resources
----------------------

- [Getting Started with Vagrant](http://docs.vagrantup.com/v2/getting-started/index.html)
- [Creating a base box](http://docs.vagrantup.com/v2/boxes/base.html)
- [Ansible Provisioner](http://docs.vagrantup.com/v2/provisioning/ansible.html)
- [Using Vagrant and Ansible](http://docs.ansible.com/guide_vagrant.html)
