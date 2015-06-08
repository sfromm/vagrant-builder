vagrant-builder
===============

This is intended to set up a workstation for vagrant and build base
boxes for use with vagrant.  At this time, **vagrant-setup.yml** assumes
it is setting up a Fedora 21 host for use with Vagrant and Libvirt.

There are two ansible playbooks:

- **vagrant-setup.yml**:  Installs the necessary packages and does other
   actions that requires root to prepare the workstation.
- **vagrant-builder.yml**: Copies configuration files to *~/builder* and
   runs *virt-builder* to create boxes, as defined in inventory.

General usage:

    ansible-playbook vagrant-setup.yml
    ansible-playbook vagrant-builder.yml

Most variables are defined in inventory.  Update them there or overrride them.

Customization
-------------

There are two areas where you can customize the build of your box:

- The variable **extra_virt_builder_options**.  Add options to pass to
  *virt-builder* here.  This variables is defined via inventory.
- Customize *bootstrap.yml*, the playbook that is run when the box is customized.

Varible definitions
-------------------

Unless the variable is defined above, be cautious in changing these
variables.

- **vagrant_pkgs**: List of packages to install to use vagrant.
  Defaults to `vagrant`, `vagrant-libvirt`, `libvirt`,
  `libvirt-daemon-kvm`, and `libguestfs-tools`.
- **user_home**: Value of `$HOME`.
- **builder_dir**: Where to build the boxes.  Defaults to
  `$HOME/builder`.
- **format**: Format for image.  Defaults to `qcow2`.
- **size**: Size of image.  Defaults to `10`G.
- **vagrant_metadata**:  A JSON string used for *metadata.json* that
  defines format, size, and provider.
- **vagrant_pass**:  Encrypted password for *vagrant* user.  Defaults to
  `vagrant`.
- **virt_builder_options**:  Options passed to *virt-builder* when
  creating the image.
- **extra_virt_builder_options**:  Additional options to pass to
  *virt_builder*.  Please use this for customization.
- **virt_builder_images**: List of images to build for vagrant.
  Presently defaults to `centos-6` and `centos-7.1`.

Miscellaneous resources
----------------------

- [Getting Started with Vagrant](http://docs.vagrantup.com/v2/getting-started/index.html)
- [Creating a base box](http://docs.vagrantup.com/v2/boxes/base.html)
- [Ansible Provisioner](http://docs.vagrantup.com/v2/provisioning/ansible.html)
- [Using Vagrant and Ansible](http://docs.ansible.com/guide_vagrant.html)
