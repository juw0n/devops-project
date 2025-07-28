### AUTOMATING AND TESTING A HOST- BASED FIREWALL

Opening ports for services like SSH or web servers is a required and acceptable risk for software or DevOps developers. That does not imply, however, that we should disregard any additional traffic going to our host. We must filter therefore, we employ firewalls to keep an eye on all packets coming in and leaving a network or host.

There are two types of firewalls:
1. host-based firewall which manages the packets entering and leaving a single host, whereas a 
2. Network firewall which is typically a device that handles all traffic moving between networks.

for this chapter, i will use a software application called `Uncomplicated Firewall (UFW)`. This firewall will block all inbound traffic except SSH connections and the Greeting web application I installed in the previous chapter.

### Planning the Firewall Rules
Firewall rules need to be very explicit about what traffic to permit and what traffic to deny.
In FW, I can divide the firewall traffic flow into three default parts, called `chains`, which is a door through which a packets must pass to be delivered to a specific place when properly routed.

The three default chains in UFW:
- **Input chain** Filters packets destined for the host
- **Output chain** Filters packets originating from the host
- **Forward chain** Filters packets that are being routed through the host

The firewall rules I'll create will only be for the input chain, because I’m focusing on the inbound traffic to the VM. It will allow incoming traffic for two known ports while rejecting all others. Port 22 for shell access (SSH) and Ansible provisioning; and port 5000 for the web application. I’ll also add rate limiting to port 5000, to protect the web server and host from excessive abuse.
Finally, I’ll enable the firewall log so I can audit the network traffic that comes through the firewall on the VM.

### UFW Firewall Automation with Ansible
Uncomplicated Firewall (UFW) is a user-friendly interface for managing firewall rules, built on top of the complex `iptables framework` (which includes Netfilter, connection tracking, and NAT). UFW simplifies host-based firewall configuration.

Using Ansible, UFW rules can be created in a simple, repeatable, and automated manner, making it easy to secure systems at scale without manually dealing with iptables syntax.

You can set the logging parameter to off, low, medium, high, or full. 
* The low log level will log any blocked packets that do not match the default policy and any other configured firewall rules. 
* The medium level does everything the low level does, plus it logs all allowed packets that do not match the default policy and all new connections. 
* The high log level does everything the medium does, but it also logs all packets with some rate limiting of the messages.

PS: Any setting above medium will generate a lot of log data and could fill up disks fast on a busy host, so be careful with those log settings.

SSH Uncomplicated Firewall (UFW) setting
Ansible ufw module can be use to creates a rule that allows an incoming connection from any source IP address, using the TCP transport protocol to port 22 on a VM. You can set the rule parameter to deny, limit, or reject,

if you want to stop a connection on a specific port but don’t mind sending a rejection reply to the remote host, you should choose reject. The rejection reply will tell the remote system that you are up and running but not accepting traffic on that port.

If you want to drop the incoming packet on the floor without any reply to the remote host, choose a deny rule. This can make it harder for someone scanning your host to know if the host is up and running.

The default rate-limiting feature for UFW states is six connections
in a 30-second time span. To adjust the default rate limit setting, create a new task using the `lineinfile module` to locate and update the line in `/etc/ufw/user`. rules
 
For example:
```
-A ufw-user-input -p tcp --dport 5000 -m conntrack --ctstate NEW -m recent --update --seconds
30 --hitcount 6 -j ufw-user-limit
```
Change the hitcount and seconds options to whatever makes sense for your environment.

### Testing the Firewall
After uncommenting the chp-5 playbook line in the site.yml file, i re-provisioned the VM:
```
 vagrant provision
```
The total complete tasks should be about 26.
### Testing the Firewall
i test that my host-based firewall is enabled by accessing the VM from my local host, through thr VM IP.
I signed in as jaywon, then grap the VM IP with the `ip command`
```
ssh -i ~/.ssh/dev_vm -p 2222 jaywon@localhost
```
```
jaywon@dev-vm:~$ ip -4 -br addr
```

### Scanning Ports with Nmap
To test that the firewall is filtering traffic, i use the nmap (network mapper) command line tool for scanning hosts and networks. 
On my local host:
```
$ sudo nmap -F <VM ip address>
```
The -F flag tells nmap to do a fast scan, which looks for only the 100 most common ports
```
$ sudo nmap -sV <VM ip address>
```
The -sV flag tells nmap to attempt to extract service and version information from running services.

### Firewall Logging
I enabled logging and set the level to `low` for `UFW` in the Ansible task. The log for those events is located in the `/var/log/ufw.log` file. This logfile requires root permissions to read it, I switched to `vagrant` user to read it using `sudo su` command. I then observered the logs for traffic that was allowed and those that was blocked.

### Rate Limiting
To test that the firewall will rate-limit excessive connection attempts (6 in 30 seconds) to my Greeting web server, I leverage the curl command with a simple for loop in Bash to iterates and executes the curl command six times in succession. 
On the host machine terminal
```
for i in `seq 1 6` ; do curl -w "\n" http://VM-IP Address:5000 ; done
```

### Conclusion
I implemented a simple but effective host-based firewall on a ubuntu VM.