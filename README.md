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

The VagrantFile in this project contains configurations to provision a loadbalancer and 2 webservers.
ansible_local vagrant provisioner is being used to run the playbooks which will install NGINX on each machine.

**Ansible roles:**
*common* - holds tasks and handlers which is common to both load balancer and web server. It installs nginx 

*loadbalancer* - task and handlers for loadbalancers. It also has config template for ngnix loadbalancer setup.

*web* - task and handlers for web servers. It also has sample html and config template for nginx webserver setup.

Ansible playbook, playbook_web provisions the created web servers by installing, nginx and copy index.html template to webnodes. 

Ansible playbook ,playbook_lb provisions the load balancer and also updates the config file for load balancer based on _number of web nodes created. It uses facts from ansible to get the ip address of the hosts and add them in load balancer config of nginx.

Clone this project, then run `vagrant up` on the terminal

This will start the VM, and run the provisioning playbook. To re-run a playbook on an existing VM, just run:

`vagrant provision`


The application for running the "hello world" test is in python3.
We can run ` $ python3 -m http.server <forwarded_port> ` to start up a web server that serves the directory in which the command was ran.
create an index.html file  in the home directory of the VMs and type `Hello,World!` in it. Save and close the file.

Restart the python server and go to `http://localhost:<forwarded_port of each VM>` you should see Hello,World! on your browser.

*N.B* To set python 3 as default on the ubuntu VM's;
    Open your .bashrc file on each VM, `vi ~/.bashrc.`
    Type `alias python=python3` on to a new line at the top of the file then save and close the file with `:wq!`
    Then, back at your command line type `source ~/.bashrc` Now your alias should be permanent.

**Making nginx idempotent**

- By using ansible apt module to install the nginx server , this step is idempotent by default as apt-get will not install nginx again once it's installed once

**Challenges**
- I had challenges running the playbooks from the Vagrantfile, Error: connection refused 
- Nginx not getting restarted via ansible: ansible is somehow skipping the restart nginx command
- I could not come up with an automated test to test the app and loadbalancer

**Remarks**

 I am quite familiar with ansible, but this is first time I used Vagrant and I thoroughly enjoyed learning it. 


