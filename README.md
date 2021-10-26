# Shecurity

This repo contains all the tools needed to reproduce the demo shown during the Shecurity talk "Blue Burns Brighter: How to Manage Burnout and Keep your SOC Happy"


## Building a Test Environment 

Technologies used:

- [https://www.vagrantup.com/intro] [Vagrant]: This tool is used for building and managing virtual machine environments in a single workflow
- [https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html] [Ansible]: tool used for provisioning, configuration management, and application deployment enabling infrastructure as code

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

You will need to make sure Ansible is installed and if not you will need to install Ansible. 

Please follow these directions to install Ansible: https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html

The ansible playbook installs and configures osquery onto the virtual box. 

The playbook is called "playbook.yml" -> ansible used yml (yet another markdown language). 

The playbook installs osquery onto the box, adds the needed configuration files in the correct locations, and start the agent on the box so it is running. The "name" section in the playbook explains the action the play book will be taking. 

To run the playbook use the following command:

ansible-playbook playbook.yml -i hosts 

NOTE: Do not forget to add your private key path to the hosts file so you will be able to SSH into the box. Double check the host file information matches the information for YOUR box. Once this runs successfully you should have osquery installed and working on your test box. 

### Validate Osquery is working on the test box

1. Vagrant SSH -> SSH into the box

2. Run the following command: sudo osqueryd status
    - This will tell you if osquery is running on the box. You may see some error messages but should not effect osquery working
3. cd etc/osquery -> Here you will find all the configuration files. 
    - osquery.flags -> configures osquery
    - osquery.conf -> tells osquery what to monitor and look for. Gives all the queries. 
    
    To keep playing around with osquery please read the documentation into all the cool things you can monitor and detect using the agent!

4. cd var/log/osquery -> here are where the osquery logs are located
    - The logs you are about them most are the osquery.results.log -> here is where you will see the file monitored events, process events, and socket events. Don't forget to take actions that osquery is monitoring so there are logs to look at. 


Now you have your test environment create with osquery :) HAVE FUN!
    


