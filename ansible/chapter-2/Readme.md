### USING ANSIBLE TO MANAGE PASSWORDS, USERS, AND GROUPS
Users and passwords are the building blocks of identity management, while groups allow you to manage a collection of users and control access to files, directories, and commands.

#### Enforcing Complex Passwords

Enforcing complicated passwords on all hosts that users can access is necessary because letting users choose what constitutes a strong password can lead to trouble.
We use code to enforce strong passwords for all users.

Ansible modules basically perform common actions on an OS, such as enabling a firewall, managing users, or installing software.

To ensure that every user has a strong password, we employ code. To accomplish this, we can install a plug-in called Pluggable Authentication Modules (PAM), a user authentication framework used by the majority of Linux distributions, using an Ansible task.
The PAM module's pam_pwquality plug-in is in charge of creating complex passwords. This module verifies passwords according to your specified criteria.

Ansible tasks should start with a name declaration that defines their goal. Then the Ansible package module, which performs the software installation. The package module usually requires you to set two parameters: name and state. The package name (which must be present in the Linux Distro repository, e.g., found in the Ubuntu repository) and the state should be present.
To remove a package, set the state to absent. If you install the package (present) and then delete the task from Ansible, the package will still be installed on the next provision. You would have to explicitly set the package to absent if you wanted the host to represent your desired state.

#### Configuring pam_pwquality to Enforce a Stricter Password Policy

A file named /etc/security/pwquality.conf (Policy Configuration) handles configuration of the pam_pwquality module.
This file is where the Ansible task makes the necessary changes to validate passwords. All you need to do is change one line in that file. A common way to edit a line using Ansible is with the lineinfile module, which allows you to change a line in a file or check whether a line exists.



Tasks in ansible are run in order, for example you need to make sure that a group exists first before making reference to the group. If you tried to reference the group before creating it, you would get an error message stating that the developers group doesn’t exist, and the provisioning would fail.