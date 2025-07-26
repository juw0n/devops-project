### CONTROLLING USER COMMANDS WITH SUDO

The chapter basically teach how to automate application deployment and control it with a sudoers policy.

On Linux operating systems, the sudo (superuser do) command allows a user or group of users to run specific commands as root or another user while keeping an audit trail of events.
Sudo can be enhanced with plug-ins. In fact, sudo comes with a default security policy plug-in called **_sudoers_**, which determines what permissions users have when they invoke the sudo command.

A sudoers file is the place where you configure security policies(for users and groups) that invoke the sudo command. This type of security file is composed of sections called Defaults, User Specifications, and Aliases.
* The Defaults syntax allows you to override some sudoers options at runtime, such as setting environment variables that users have access to when they run sudo.
* The User Specifications section determines which commands users can run and on which host they can run them.
* The Aliases syntax references other objects inside the file, and
that is useful for keeping the configuration clear and concise when there is a lot of duplication.

> The four aliases you can mix and match are as follows:
    - `Host_Alias:` A host or a group of hosts
    - `Runas_Alias:` A list of users or groups a command can be run as
    - `Cmnd_Alias:` Specifies a command or multiple commands
    - `User_Alias:` A user or group of users

The sudoedit command (or sudo -e) is designed to allow users to edit files with elevated privileges (e.g., as root) while enforcing strict security policies to prevent misuse or privilege escalation.
PS: Read about sudoedit's security policy

One of the policies that help in controlling user command with sudo to prevent security risks:

1. Directory and File Ownership Checks
Directory Ownership: The parent directory of the file being edited must not be writable by the current user. If the directory is writable, sudoedit will block the edit to prevent tampering.

2. File Ownership: The file itself must not be writable by the current user. If the file is writable, sudoedit will block the edit.
(The directories and files should be owned by root or another privileged user)

#### Provisioning the VM
To run all the tasks for this chapter, I uncommented them in the playbook i.e. site.yaml file like you I in previous chapters.

re-provision the VM
```
 vagrant provision
```
There should be a total of `21 ok tasks`
#### Testing Permissions
ssh to jaywon using passphrase and verification code from chp-3
```
ssh -i ~/.ssh/dev_vm -p 2222 jaywon@localhost
```
- Accessing the Web Application
```
curl http://localhost:5000
```
- Editing greeting.py to Test the sudoers Policy
```
sudoedit /opt/engineering/greeting.py
```
* Stopping and Starting with systemctl
```sudo systemctl stop greeting```
```sudo systemctl start greeting```
- Accessing the Web Application again
```
curl http://localhost:5000
```

#### Audit Logs
The sudo events were captured in the /var/log/auth.log file.
> The auth log requires elevated permissions to read, and since the sudoers policy does not grant that to **_jaywon,_** you’ll need to issue sudo as the vagrant user to view it. 


#### Conclusion:
In this chapter, I explored the importance of allowing users to run commands with elevated privileges in a controlled and auditable way. Using Ansible, the sudo command, and a well-configured sudoers file, I demonstrated how to:
* Restrict command access for specific user groups
* Allow privilege escalation without exposing full root access
* Maintain an audit trail for better security and accountability

I also worked with various Ansible modules, including:

* 🧩 template – Used to create files dynamically by rendering Jinja2 templates with variables.
* ⚙️ systemd – Managed application services (start, stop, restart) to control the application’s lifecycle.
* 📌 set_fact – Set host-specific variables at runtime for flexible and dynamic playbook execution.

These tools allowed me to automate the installation and lifecycle management of a web application in a secure, scalable, and repeatable manner.