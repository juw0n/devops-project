### USING ANSIBLE TO CONFIGURE SSH

SSH is a protocol and tool that provides command line access for managing a remote host or a fleet of remote hosts mostly headless, from a local machine.

Since SSH opens access to a host, misconfiguration or default installations can lead to unauthorized access.

#### Use Ansible to secure SSH access to a VM
1. Disable password access over SSH
2. Use public key authentication over SSH
3. Enable two-factor authentication (2FA) over SSH

#### Understanding and Activating Public Key Authentication
This method uses a key pair, consisting of a public key file and a private key file, to confirm identity. Public key authentication is considered best practice for authenticating users over SSH because potential attackers who want to hijack a user’s identity need both a copy of a user’s private key and the passphrase to unlock it.

When an SSH session is created with a key, the remote host encrypts a
challenge with the public key and sends the challenge back to the local host. Because the local host is in possession of the private key, it can decode the message and send back a response to the remote server. If the server can validate the response, it will know that the local host is in possession of the private key and will thus confirm your identity.

#### Generating a Public Key Pair
To generate a key pair, you’ll use the ssh-keygen command line tool. This tool, is usually installed on Unix hosts by default as part of the ssh package, to generates and manages authentication key pairs for SSH.

A passphrase is like a password, but it’s usually longer (more like a group of unrelated words than a complex stream of characters).

==> $ ssh-keygen -t rsa -f ~/.ssh/devops_vm -C devops_vm

ssh-keygen is to create an rsa key pair that has a name of devops_vm inside ~/.ssh/. if no name was specified, it will default to "id_rsa". 
The -C flag adds a human-readable comment to the end of the key that can help identify what the key is for, in this case devops_vm

#### Using Ansible to Get Your Public Key on the VM
To copy the ssh public key to the remote server with Ansible, we use the authorized_key module. This module is use to copy your public key from the local host over to a user on the VM.

#### Adding Two-Factor Authentication
Security is built in layers. The more layers you have, the harder it is for an intruder to gain access. which is why we will be adding two-factor authentication (2FA), which validates a user’s identity by using credentials and something that the user has. The main goal of 2FA is to make it harder for someone identity if password or key is compromised.
Two-factor authentication relies on the user providing two out of these three things: 
==> Something they know: password or pin
==> Something they have: phone or hardware authentication device (Software or Hardware)
==> Something they are: fingerprint or voice (Biometric)