# Nftables-for-Debian-Desktop

```bash
This repo contains the rules for nftables for desktop. 
The file " nftables rules " contains the rules that is to be typed  in the terminal. 
The file " nftables ruleset.md " contains the nftables script that is to be started at 
boot time using systemd. 
```

_Enabling the nftables at boot time_

```bash
systemctl enable nftables
 
Remove the Execstop in the nftable.service file. The file is usually located in 
 
/etc/systemd/system/sysinit.target.wants/nftables.service

```
 
