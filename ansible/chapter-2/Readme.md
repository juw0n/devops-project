### USING ANSIBLE TO MANAGE PASSWORDS, USERS, AND GROUPS
Managing users, passwords, and groups is fundamental to system identity and access control. Groups simplify permissions management by grouping users and assigning collective access to files, directories, and commands.

#### Enforcing Complex Passwords

Letting users create their own passwords can lead to weak security.
We use _Ansible automation_ to enforce strong, complex passwords across all hosts.

Ansible provides modules, which are code units that interact with system resources or execute system commands on an OS, such as enabling a firewall, managing users, or installing software. Ansible provides a module library that you can execute directly on remote hosts or through playbooks.
Common modules include:
- package – for software installation
- lineinfile – for editing files line-by-line
- copy, user, group, file, and apt – for system management

Similar to modules are plugins, which are pieces of code that extend core Ansible functionality which enable a rich, flexible, and expandable feature set.

#### ⚙️ PAM & pam_pwquality
To enforce password complexity, we configure Pluggable Authentication Modules (PAM) using Ansible. we install a linux plug-in called ***Pluggable Authentication Modules (PAM)***, a user authentication framework used by the majority of Linux distributions, using an Ansible task.
The pam_pwquality plugin, part of the PAM stack, verifies password strength according to defined policies or specified criteria.

Ansible tasks should start with a name declaration that defines their goal. Then the Ansible package module, which performs the software installation. The package module usually requires to set two parameters: **name and state**. The package name (which must be present in the Linux Distro repository, i.e., found in the Ubuntu repository) and the state should be present.
***To remove a package, set the state to absent***.

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

To create the SHA-512 hash for Ansible’s user module, i used the following command on the Host Ubuntu:
```
sudo apt update
sudo apt install pwgen whois
pass=`pwgen --secure --capitalize --numerals --symbols 12 1`
echo $pass | mkpasswd --stdin --method=sha-512; echo $pass
```

The last command display two output lines,
* The first output is the SHA-512 hash
* The second is the plaintext password

> Use the hash in your Ansible task.

PS: the the site.yml file, uncomment the chp-2 lines so Ansible can execute the tasks.

#### Execution
1.Start the VM (if off): navigate the the vagrant folder and run
```
vagrant up
```
💡 Ensure that chp-2 tasks in site.yml are uncommented before running provision.
2. Re-provision the VM (if already running):
```
vagrant provision
```
to update the VM by executing the ansible tasks.

#### Verifying User, Group, and Permissions
SSH or log into the VM:
 ```
vagrant ssh
```
You should be logged in as the vagrant user, which is the default user
Vagrant creates.

Check if the user and group were created:
```
getent passwd jaywon
getent group dev-team
```
Try accessing restricted files as vagrant user:
```
ls -al /opt/engineering/
cat /opt/engineering/private-info.txt
```
You should see "Permission denied."
Switch to the authorized user:
```
sudo su - jaywon
```
Now retry the file access, it should succeed.