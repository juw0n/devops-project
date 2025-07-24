### USING ANSIBLE TO MANAGE PASSWORDS, USERS, AND GROUPS
Users and passwords are the building blocks of identity management, while groups allow you to manage a collection of users and control access to files, directories, and commands.

#### Enforcing Complex Passwords

Enforcing complicated passwords on all hosts that users can access is necessary because letting users choose a strong password can lead to trouble.
We use code to enforce strong passwords for all users.

Ansible modules are units of code that can control system resources or execute system commands on an OS, such as enabling a firewall, managing users, or installing software. Ansible provides a module library that you can execute directly on remote hosts or through playbooks.
e.g., `modules are`: package, lineinfile, copy, apt (for ubuntu), user, group, and file etc

Similar to modules are plugins, which are pieces of code that extend core Ansible functionality. Ansible uses a plugin architecture to enable a rich, flexible, and expandable feature set.

To ensure that every user has a strong password, we employ code. To accomplish this, we install a linux plug-in called ***Pluggable Authentication Modules (PAM)***, a user authentication framework used by the majority of Linux distributions, using an Ansible task.
The PAM module's pam_pwquality plug-in is in charge of creating complex passwords. This module verifies passwords according to specified criteria.

Ansible tasks should start with a name declaration that defines their goal. Then the Ansible package module, which performs the software installation. The package module usually requires to set two parameters: **name and state**. The package name (which must be present in the Linux Distro repository, i.e., found in the Ubuntu repository) and the state should be present.
***To remove a package, set the state to absent***. If you install the package (present) and then delete the task from Ansible, the package will still be installed on the next provision. You would have to explicitly set the package to absent if you wanted the host to represent your desired state.

#### Configuring pam_pwquality to Enforce a Stricter Password Policy

A file named /etc/security/pwquality.conf (Policy Configuration) handles the configuration of the pam_pwquality module.
This file is where the Ansible task makes the necessary changes to validate passwords. All you need to do is change one line in that file. A common way to edit a line using Ansible is with the `lineinfile module`, which allows you to change a line in a file or check whether a line exists.


Tasks in Ansible are run in order, for example, you need to make sure that a group exists first before making reference to the group. If you tried to reference the group before creating it, you would get an error message stating that the devops group doesn’t exist, and the provisioning would fail.

> PS: The negative values in the configuration line above inform pam_pwquality that it must have at least “one of” for that category. 

#### Ansible User Module
In Linux systems, there are three types of user
1. Normal User: Typically a human account with a username, password and belongs to a group or groups. User ID usually above 1000
2. System User: The non-human accounts used by services on the system. e.g., Nginx, and Docker system accounts. their user ID is usually below 1000
3. Root user: A root user (or superuser) account has unrestricted access to the operating system. It usually has a UID of zero(0)

To create and manage users and groups using Ansible, you use the User and group Module

#### Password generation
I used a combination of two command line applications, pwgen and mkpasswd, to create the complex password. The pwgen command can generate secure passwords, and the mkpasswd command can generate passwords using different hashing algorithms. The pwgen application is provided by the pwgen package, and the mkpasswd application is provided by a package named whois. Together, these tools can generate the hash that Ansible and Linux expect.

Linux stores password hashes in a file called shadow. On an Ubuntu system, the password hashing algorithm is SHA-512 by default. To create the SHA-512 hash for Ansible’s user module, i used the following command on the Host Ubuntu:

==> $ sudo apt update
==> $ sudo apt install pwgen whois
==> $ pass=`pwgen --secure --capitalize --numerals --symbols 12 1`
==> $ echo $pass | mkpasswd --stdin --method=sha-512; echo $pass

The last command display two output lines, the hash password and the real password for jaywon user.

PS: the the site.yml file, uncomment the chp-2 lines so Ansible can execute the tasks.

#### Execution
if the VM is not running, navigate the the vagrant folder and run
```
vagrant up
```
or if the VM is still running, run
```
vagrant provision
```
to update the VM by executing the ansible tasks.

#### Testing User and Group Permissions
 log in to the VM:
 ```
vagrant ssh
```
You should be logged in as the vagrant user, which is the default user
Vagrant creates.

verify user was created by running
```
getent passwd jaywon
```

verify group was created by running
```
getent group developers
```
check the file permission using the vagrant user should give error
```
$ ls -al /opt/engineering/
ls: cannot open directory '/opt/engineering/': Permission denied

$ cat /opt/engineering/private.txt
cat: /opt/engineering/private.txt: Permission denied
```
To switch to the authorized user i.e jaywon, use
```
sudo su - jaywon
```
then try the file permission again