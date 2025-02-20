==========================================================================
Chapter 1 - Setting Up virtual Machine

## Provisioning (that is, setting up) a virtual machine (VM) is the act of configuring a VM for a specific purpose. Setting up a VM requires two steps: 
1. Creating VM - Tools: Vagrant
2. Configuring VM. Tools: Ansible

Infrastructure as code (IaC): It is the process of using code to describe and manage infrastructure like VMs, network switches, and cloud resources such as Amazon Relational Database Service (RDS). e.g of tools are: vagrant

Configuration Management (CM): It is the process of configuring those resources for a specific purpose in a predictable, repeatable manner. e.g of tools are: Ansible


A Vagrantfile describes how to build and provision a VM. It’s best practice
to use one Vagrantfile per project so you can add the configuration file to
your project’s version control and share it with your team.

### Basic Vagrant Commands:
1. vagrant up  Creates a VM using the Vagrantfile as a guide
2. vagrant destroy  Destroys the running VM
3. vagrant status  Checks the running status of a VM
4. vagrant ssh  Accesses the VM over Secure Shell

Get the vagrantFile for ubuntu jammy64 by running
vagrant init ubuntu/jammy64 --box-version 20241002.0.0

### Basic Ansible terms:
1. Playbook  A playbook is a collection of ordered tasks or roles that you
can use to configure hosts.
2. Control node   A control node is any Unix machine that has Ansible
installed on it. You will run your playbooks or commands from a control
node, and you can have as many control nodes as you like.
3. Managed node: A remote system, or host, that Ansible controls.
4. Inventory  An inventory is a file that contains a list of hosts or groups of
hosts that Ansible can communicate with.
Module  A module encapsulates the details of how to perform certain
actions across operating systems, such as how to install a software pack-
age. Ansible comes preloaded with many modules.
5. Task  A task is a command or action (such as installing software or
adding a user) that is executed on the managed host.
6. Role  A role is a group of tasks and variables that is organized in a
standardized directory structure, defines a particular purpose for the
server, and can be shared with other users for a common goal. A typi-
cal role could configure a host to be a database server. This role would
include all the files and instructions necessary to install the database
application, configure user permissions, and apply seed data.