vagrant-builder
===============

This is intended to set up a workstation for vagrant.  At this time,
that is limited to Fedora 20.  The ansible playbook is divided into two
plays:

- The first installs necessary packages and does other actions that
  require root.
- The second play copies configuration files to ~/builder/ and runs
  *virt-builder* to create boxes.

You can then run the playbook with *--start-at-task*.

General usage:

    ansible-playbook vagrant-builder.yml
    ansible-playbook vagrant-builder.yml --start-at-task 'create directories'

Most variables are defined in inventory.  Update them there or overrride them.
