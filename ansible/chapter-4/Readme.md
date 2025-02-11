### CONTROLLING USER COMMANDS WITH SUDO

The chapter basically teach how to automate application deployment and control it with a sudoers policy.

On Linux operating systems, the sudo (superuser do) command allows a user or group of users to run specific commands as root or another user while keeping an audit trail of events.
Sudo can be enhanced with plug-ins. In fact, sudo comes with a default security policy plug-in called sudoers, which determines what permissions users have when they invoke the sudo command.

A sudoers file is the place where you configure security policies(for users and groups) that invoke the sudo command. This type of security file is composed of sections called Defaults, User Specifications, and Aliases.
* The Defaults syntax allows you to override some sudoers options at runtime, such as setting environment variables that users have access to when they run sudo.
* The User Specifications section determines which commands users can run and on which host they can run them.
* The Aliases syntax references other objects inside the file, and
that is useful for keeping the configuration clear and concise when there is a lot of duplication.
==> The four aliases you can mix and match are as follows:
=> Host_Alias  A host or a group of hosts
=> Runas_Alias  A list of users or groups a command can be run as
=> Cmnd_Alias  Specifies a command or multiple commands
=> User_Alias  A user or group of users

The sudoedit command (or sudo -e) is designed to allow users to edit files with elevated privileges (e.g., as root) while enforcing strict security policies to prevent misuse or privilege escalation.
PS: Read about sudoedit's security policy

One of the policies that help in controlling user command with sudo to prevent security risks:

1. Directory and File Ownership Checks
Directory Ownership: The parent directory of the file being edited must not be writable by the current user. If the directory is writable, sudoedit will block the edit to prevent tampering.

2. File Ownership: The file itself must not be writable by the current user. If the file is writable, sudoedit will block the edit.
(The directories and files should be owned by root or another privileged user)

Conclusion:
I explored the importance of allowing users to run commands with elevated privileges. Using Ansible, the sudo command, and a sudoers file, you can restrict command access and log an audit trail for security.
I also worked with some different Ansible modules like template (The Ansible template module is useful for creating files that will require some modification with variables. The template module creates files using the Jinja2 template engine for Python templates.), systemd (This module is used for checking and modifying system and applications services), and set_fact (This module allows the setting of host variables that can be used in a task or across a playbook.), which allowed you to automate the installation of your web applica-
tion and control its life cycle.