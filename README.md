# Nftables-for-Debian-Desktop

```bash
This repo contains the rules for nftables for desktop. 
The file " nftables rules " contains the rules that is to be typed  in the terminal. 
The file " nftables ruleset.md " contains the nftables script that is to be started at 
boot time using systemd. 
If you want to include ipv6 protocol in your firewall use the inet address family instead of ip 
as it combines both ip and ip6 protocols in nftables.
```

_Enabling the nftables at boot time_

```bash
systemctl enable nftables
 
Remove the Execstop in the nftable.service file. The file is usually located in 
 
/etc/systemd/system/sysinit.target.wants/nftables.service

```
_ Logging invalid packets by rsyslog_

```bash
Create a directory called nftables in /var/log and in it, create two files called
Input.log and Output.log to store the input and output logs, respectively. 
Now go to /etc/rsyslog.d and create a file called nftables.conf with the following contents:

:msg,regex, " Invalid-Input: " -/var/log/nftables/Input.log
:msg,regex, " Invalid-Output: "  -/var/log/nftables/Output.log
& stop

Save the file and exit. Now all the log for nftables will be saved in these files

```
_Reference_

[Logging in Nftables](https://www.lastbreach.de/blog/firewall-logging-mit-nftables-und-rsyslog)
