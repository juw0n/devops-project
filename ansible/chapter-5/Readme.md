### AUTOMATING AND TESTING A HOST- BASED FIREWALL

Opening ports for services like SSH or web servers is a required and acceptable risk for software or DevOps developers. That does not imply, however, that we should disregard any additional traffic going to our host. We must filter all other traffic and make informed decisions about what goes in and out in order to minimise threats. As a result, we employ firewalls to keep an eye on all packets coming in and leave a network or server.

There are two types of firewalls:
1. host-based firewall which manages the packets entering and leaving a single host, whereas a 
2. Network firewall which is typically a device that handles all traffic moving between networks.

You can set the logging parameter to off, low, medium, high, or full. 
1. The low log level will log any blocked packets that do not match your default policy and any other firewall rules you have added. 
2. The medium level does everything the low level does, plus it logs all allowed packets that do not match the default policy and all new connections. 
3. The high log level does everything the medium does, but it also logs all packets with some rate limiting of the messages.

PS: Any setting above medium will generate a lot of log data and could fill up disks fast on a busy host, so be careful with those log settings.

SSH Uncomplicated Firewall (UFW) setting
Ansible ufw module can be use to creates a rule that allows an incoming connection from any source IP address, using the TCP transport protocol to port 22 on a VM. You can set the rule parameter to deny, limit, or reject,

if you want to stop a connection on a specific port but don’t mind sending a rejection reply to the remote host, you should choose reject. The rejection reply will tell the remote system that you are up and running but not accepting traffic on that port.

If you want to drop the incoming packet on the floor without any reply to the remote host, choose a deny rule. This can make it harder for someone scanning your host to know if the host is up and running.