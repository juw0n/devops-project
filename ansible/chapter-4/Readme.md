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