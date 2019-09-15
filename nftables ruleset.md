```bash

# load the script from file at startup using systemd 

table ip filter {
	chain input {
		type filter hook input priority 0; policy drop;
		iifname "lo" accept					
		iifname "enp2s0" tcp flags != syn ct state new drop
		iifname "enp2s0" ct state established,related,new accept
		iifname "enp2s0" udp length { 28-32 } drop
		iifname "enp2s0" ip saddr 10.0.0.0/8 drop
		iifname "enp2s0" ip saddr 172.16.0.0/12 drop
		iifname "enp2s0" ip saddr 192.168.0.0/16 drop
		iifname "enp2s0" ip saddr 224.0.0.0/4 drop
		iifname "enp2s0" ip daddr 224.0.0.0/4 ip protocol != udp drop
		iifname "enp2s0" ip saddr 240.0.0.0/5 drop
		iifname "enp2s0" ip saddr 127.0.0.0/8 drop
		iifname "enp2s0" ip saddr 255.255.255.255 drop
		iifname "enp2s0" ip daddr 255.255.255.255 drop
		iifname "enp2s0" ip saddr 169.254.0.0/16 drop
		iifname "enp2s0" ip saddr 0.0.0.0/8 drop
		iifname "enp2s0" ip daddr 0.0.0.0/8 drop
		iifname "enp2s0" tcp flags & (fin | syn) == fin | syn drop
		iifname "enp2s0" tcp flags & (syn | rst) == syn | rst drop
		iifname "enp2s0" tcp flags & (fin | rst) == fin | rst drop
		iifname "enp2s0" tcp flags & (fin | ack) == fin drop
		iifname "enp2s0" tcp flags & (psh | ack) == psh drop
		iifname "enp2s0" tcp flags & (ack | urg) == urg drop
		iifname "enp2s0" tcp flags & (fin | syn | rst | psh | ack | urg) < fin drop
		iifname "enp2s0" tcp flags & (fin | syn | rst | psh | ack | urg) == fin | psh | urg drop
		iifname "enp2s0" tcp flags & (fin | syn | rst | psh | ack | urg) == fin | syn | rst | ack drop
		iifname "enp2s0" tcp flags & (fin | syn | rst | psh | ack | urg) == fin | syn | rst | psh | ack | urg drop
		iifname "enp2s0" tcp flags & (fin | syn | rst | psh | ack | urg) == fin | syn | rst | ack | urg drop
		iifname "enp2s0" tcp flags & (fin | syn | rst | psh | ack | urg) == fin drop
		iifname "enp2s0" tcp flags & (fin | syn | rst | ack) == rst drop
		iifname "enp2s0" tcp sport { 6000-6063 } ct state new drop
		iifname "enp2s0" tcp sport { socks, cisco-sccp, nfs, 3128 } ct state new drop
		iifname "enp2s0" udp sport { socks, cisco-sccp, nfs, 3128, 4045 } ct state new drop
		iifname "enp2s0" icmp type { echo-reply, destination-unreachable, time-exceeded } limit rate 1/second accept
		iifname "enp2s0" icmp type { source-quench, redirect, echo-request, parameter-problem, timestamp-request } drop
		iifname "enp2s0" icmp type { timestamp-reply, info-request, info-reply, address-mask-request } drop
		iifname "enp2s0" icmp type { router-advertisement, router-solicitation, address-mask-reply } drop
		iifname "enp2s0" ct state invalid,untracked log prefix "Invalid-Input: " level info flags all
		iifname "enp2s0" ct state invalid,untracked drop
	}

	chain forward {
		type filter hook forward priority 0; policy drop;
		ct state established,related accept
		ct state invalid,untracked drop
	}

	chain output {
		type filter hook output priority 0; policy drop;
		oifname "lo" accept
		oifname "enp2s0" tcp flags != syn ct state new drop
		oifname "enp2s0" ct state established,related,new accept
		oifname "enp2s0" tcp dport { 6000-6063 } ct state new drop
		oifname "enp2s0" tcp dport { socks, cisco-sccp, nfs, 3128 } ct state new drop
		oifname "enp2s0" udp dport { socks, cisco-sccp, nfs, 3128, 4045 } ct state new drop
		oifname "enp2s0" icmp type { destination-unreachable, echo-request, time-exceeded } limit rate 1/second accept
		oifname "enp2s0" icmp type { echo-reply, source-quench, redirect, parameter-problem, timestamp-request } drop
		oifname "enp2s0" icmp type { timestamp-reply, info-request, info-reply, address-mask-request } drop
		oifname "enp2s0" icmp type { router-advertisement, router-solicitation, address-mask-reply } drop
		oifname "enp2s0" ct state invalid,untracked log prefix "Invalid-Output: " level info flags all
		oifname "enp2s0" ct state invalid,untracked drop
	}
}
```
