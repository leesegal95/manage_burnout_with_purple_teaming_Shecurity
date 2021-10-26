# Shecurity

This repo contains all the tools needed to reproduce the demo shown during the Shecurity talk "Blue Burns Brighter: How to Manage Burnout and Keep your SOC Happy"


## Building a Test Environment 

Technologies used:

- [https://www.vagrantup.com/intro][Vagrant]: This tool is used for building and managing virtual machine environments in a single workflow
- [https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html][Ansible]: tool used for provisioning, configuration management, and application deployment enabling infrastructure as code

### Building your Vagrant Box

1. You need to install Vagrant and its dependencies. 

    Please follow these instructions to install vagrant on your box: https://www.vagrantup.com/docs/installation

    Once Vagrant is installed can begin building our virtual environment. We want to start by building a basic ubuntu box that has an IP address (other than the local host) configured with it. 

2. Use Vagrant init command to initialize the Vagrantfile. 

    command: vagrant init ubuntu/bionic64

    This will create the bones of a vagrant file. This file has enough to spin up a box but will have no other setting configured. 

3. Add desired IP address into the Vagrantfile

    Add following line to Vagrantfile: subconfig.vm.network "private_network", ip: "192.168.33.10"
    This will now correlate 192.168.33.10 to our virtual machine. We can use this IP address when we need to run scripts against. 

4. Now you can create the box

    Vagrant up

5. You will want to check the status of the box once created

    Vagrant Status

6. You will want to see where the private key was stored and add it to in the hosts file under this section: "ansible_ssh_private_key_file:

    Vagrant ssh-config -> this will show you where the private key was stored. Copy the path and add it to the hosts file in the section mentioned above

7. You can test SSH into the box 

    Vagrant SSH 

Use the Vagrant documentation to see what other cool tricks you can do with Vagrant and building a test environment!

### Building / Running Ansible Playbook against the virtual box

The ansible playbook installs and configured osquery onto the virtual box. 
    


