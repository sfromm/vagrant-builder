vagrant-builder
===============

This is intended to set up a workstation for vagrant.  At this time,
that is limited to Fedora 20.  There are two ansible playbooks:

- *vagrant-setup.yml*:  Installs the necessary packages and does other
   actions that requires root to prepare the workstation.
- *vagrant-builder.yml*: Copies configuration files to ~/builder and
   runs *virt-builder* to create boxes, as defined in inventory.

General usage:

    ansible-playbook vagrant-builder.yml
    ansible-playbook vagrant-setup.yml

Most variables are defined in inventory.  Update them there or overrride them.
