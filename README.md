# DevOps-excercise
Building a loadbalancer and webservers infrastructure in vagrant using Ansible as a config management tool.

**This documentation aims to show how to use Ansible and Vagrant to build a load balancer, behind which runs two nginx applications which returns a simple hello world application written in python3**

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

# Solution

I Adopted a KISS approach ( keep it simple stupid ). The host box only has vagrant installed and therefore the ansible_local vagrant provisioner has been chosen.

This will install the latest version of Ansible on each host and run the playbook on the host machine.

In order to keep the solution simple no existing roles have been included, as most roles tend to do "too much" and don't solve this specific problem elegantly.
All ip addresses have been hardcoded in the vagrant file, although are dynamic when running from the inventory file making it easy to move from production to staging. 

As this is a Vagrant box, for this test environment security has not been considered. There are no firewalls / backup scripts for the box.

If this were to be moved from a test environment to a production product it would be strongly recommended to secure this system further 

Testing the application will be via the shell provisioner. 

The applications for running the "hello world" is written in python3. 

GIT has been used to version control the scripts

Everything is installed under sudo which isn't best practice but has been done for speed.



Clone this project, then run `vagrant up` on the terminal

This will start the VM, and run the provisioning playbook. To re-run a playbook on an existing VM, just run:

`vagrant provision`


**Making nginx idempotent**

- By using ansible apt module to install the nginx server , this step is idempotent by default as apt-get will not install nginx again once it's installed once

**Challenges**
- I had challenges running the playbooks from the Vagrantfile, Error: connection refused . i was able to fix that in the inventory file named vagrant-hosts
- Nginx not getting restarted via ansible: ansible is somehow skipping the restart nginx command. it was a connection error this was fixed in the inventory file also.


**Remarks**

 I am quite familiar with ansible, but this is first time I used Vagrant and I thoroughly enjoyed learning it. 


