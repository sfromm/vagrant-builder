# -*- mode: ruby -*-

# Require libvirt
Vagrant.require_plugin "vagrant-libvirt"

# Vagrantfile API/syntax version.
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

    # Every Vagrant virtual environment requires a box to build off of.
    #config.vm.define "guest" do |guest|
    #    guest.vm.box = "centos-7.0"

        # URL that a configured box can be found
    #    guest.vm.box_url = "file:///path/builder/centos-7.0"

        # Disable automatic box update checking. If you disable this, then
        # boxes will only be checked for updates when the user runs
        # `vagrant box outdated`. This is not recommended.
        # guest.vm.box_check_update = false

        # Create a forwarded port mapping which allows access to a specific port
        # within the machine from a port on the host machine. In the example below,
        # accessing "localhost:8080" will access port 80 on the guest machine.
        # config.vm.network "forwarded_port", guest: 80, host: 8080
        # Create a private network, which allows host-only access to the machine
        # using a specific IP.
        # guest.vm.network "private_network", ip: "192.168.33.10"

        # Create a public network, which generally matched to bridged network.
        # Bridged networks make the machine appear as another physical device on
        # your network.
        # guest.vm.network "public_network"
    #end



    # If true, then any SSH connections made will enable agent forwarding.
    # Default value: false
    config.ssh.forward_agent = true

    # Options for libvirt vagrant provider.
    config.vm.provider :libvirt do |libvirt|

        # A hypervisor name to access. Different drivers can be specified, but
        # this version of provider creates KVM machines only. Some examples of
        # drivers are qemu (KVM/qemu), xen (Xen hypervisor), lxc (Linux Containers),
        # esx (VMware ESX), vmwarews (VMware Workstation) and more. Refer to
        # documentation for available drivers (http://libvirt.org/drivers.html).
        libvirt.driver = 'kvm'

        # The name of the server, where libvirtd is running.
        #libvirt.host = 'localhost'

        # If use ssh tunnel to connect to Libvirt.
        libvirt.connect_via_ssh = false

        # The username and password to access Libvirt. Password is not used when
        # connecting via ssh.
        libvirt.username = 'root'
        #libvirt.password = 'secret'

        # Libvirt storage pool name, where box image and instance snapshots will
        # be stored.
        libvirt.storage_pool_name = 'default'
    end

    #config.vm.provision :ansible do |ansible|
    #    ansible.groups = { "group1" => ["guest"] }
    #    ansible.playbook = 'vagrant.yml'
    #end
end

