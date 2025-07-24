Infrastructure as Code (IaC) tool: Vagrant

Vagrant is an open-source tool used to build and manage virtual development environments. It simplifies the process of creating, configuring, and deploying virtual machines, making it easier to collaborate on projects with consistent environments across different machines.

It supports multiple operating systems (OSs) that can run on multiple
platforms, and uses a single configuration file, called a [Vagrantfile](https://developer.hashicorp.com/vagrant/docs/vagrantfile), to describe the virtual environment in code. 

### Basic Vagrant Commands
The four most often used vagrant commands are **vagrant up**, **vagrant
destroy,** **vagrant status**, and **vagrant ssh**:

```bash
    **vagrant up** Creates a VM using the Vagrantfile as a guide
    **vagrant destroy** Destroys the running VM
    **vagrant status** Checks the running status of a VM
    **vagrant ssh** Accesses the VM over Secure Shell
```

Each of these commands has additional options. To see what they are,
enter a command and then add the --help flag for more information. 