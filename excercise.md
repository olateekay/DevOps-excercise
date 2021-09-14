**This documentation aims to show a step by step method to implementing an NGINX load balancer and NGINX webservers infrastructure in vagrant using ansible as a configuration management tool.**

**Vagrant** is a tool that helps to automate the creation and management of virtual machines.

To be able to use Vagrant you need two things: 

**Vagrant itself and a hyper-visor.**

You can download Vagrant here:

https://www.vagrantup.com/downloads.html

For the hyper-visor, **VirtualBox** is a good option.

To verify that Vagrant is installed, check for the vagrant version installed:

```
> vagrant -v
Vagrant 2.2.18
```

Next stop is to create 3 VM's.First create an empty directory somewhere and `cd` into it.

Our aim is to create multiple VM's with a single Vagrantfile

*A Vagrantfile is a file that houses the configurations to create a VM or multi VM's*

Create a Vagrantfile;

`vi Vagrantfile`

Copy and paste this content into the file

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

# Every Vagrant development environment requires a box. You can search for
# boxes at https://atlas.hashicorp.com/search.
BOX_IMAGE = "ubuntu/bionic64"
NODE_COUNT = 2

Vagrant.configure("2") do |config|
  config.vm.define "master" do |subconfig|
    subconfig.vm.box = BOX_IMAGE
    subconfig.vm.hostname = "master"
    subconfig.vm.network :private_network, ip: "10.0.0.10"
  end
  
  (1..NODE_COUNT).each do |i|
    config.vm.define "node#{i}" do |subconfig|
      subconfig.vm.box = BOX_IMAGE
      subconfig.vm.hostname = "node#{i}"
      subconfig.vm.network :private_network, ip: "10.0.0.#{i + 10}"
    end
  end

  # Install avahi on all machines  
  config.vm.provision "shell", inline: <<-SHELL
    apt-get install -y avahi-daemon libnss-mdns
  SHELL
end
```

This Vagrantfile will create 3 VMs (master, node1, node2).

In the Vagrantfile, vagrantbox has been declared as a constant , BOX_IMAGE.

This configuration in the Vagrantfile;

`subconfig.vm.hostname = "a.host.name"`

gives each VM a unique hostname.

Next, we need a way of getting the IP address for a hostname. For this, we’ll use DNS – or mDNS to be more precise.

On Ubuntu, mDNS is provided by Avahi. To install Avahi on each node, we’ll use Vagrant’s provisioning feature.

Before the last end in the Vagrantfile, 

```
config.vm.provision "shell", inline: <<-SHELL
  apt-get install -y avahi-daemon libnss-mdns
SHELL
```

This will call `apt-get install -y avahi-daemon libnss-mdns` on every VM.

Last, we need to connect the VMs through a private network.

For each VM, we need to add a config like this (where each VM will have a different ip address):

`subconfig.vm.network :private_network, ip: "10.0.0.10"`

You can now call `vagrant up` and then ssh into any of the VMs:

To ssh into any of the VMs, just specify its name. For example, to ssh into `node1`, call:

`> vagrant ssh node1`

