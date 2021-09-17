# DevOps-excercise
Building a loadbalancer and webservers infrastructure in vagrant using Ansible as a config management tool.

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

Our aim is to 
- provisiom 3 VMs built using the following vagrant box:https://app.vagrantup.com/ubuntu/boxes/bionic64
- To have the VMs allow the vagrant user and users in the admin group to sudo without a password
- Have Webservers and load balancer running nginx
- Have a Simple ‘Hello World’ application deployed to both webservers (written in python3)
- Have the Solution be idempotent

The VagrantFile in this project contains configurations to provision a loadbalancer and 2 webservers.
ansible_local vagrant provisioner is being used to run the playbooks which will install NGINX on each machine.

**Ansible roles:**
*common* - holds tasks and handlers which is common to both load balancer and web server. It installs nginx 

*loadbalancer* - task and handlers for loadbalancers. It also has config template for ngnix loadbalancer setup.

*web* - task and handlers for web servers. It also has sample html and config template for nginx webserver setup.

Ansible playbook, playbook_web provisions the created web servers by installing, nginx and copy index.html template to webnodes. 

Ansible playbook ,playbook_lb provisions the load balancer and also updates the config file for load balancer based on _number of web nodes created. It uses facts from ansible to get the ip address of the hosts and add them in load balancer config of nginx.

The applications for running the "hello world" test will be written in python3.

You can now call `vagrant up` on the terminal




