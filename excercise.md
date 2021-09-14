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

