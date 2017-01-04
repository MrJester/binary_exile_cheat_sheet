---
title: TCP Dump 
category: Scanning
order: 1
---

> **TCP Dump** 

Example Command	| tcpdump -nnX tcp and port 80 and host 10.10.10.10
Protocols | ether, ip, ip6, arp, rarp, tcp, udp
Type | host, net, port, portrange
Direction | src, dst
Use numbers for machines and ports | -nn
Write to file | -w
print out in ascII or hex | -A -X
Choose interface | -i <interface>
Full packets | -s 
Network filter | Net 10.10.10

> **NMAP**

example command | ./nmap -n -A -st -p 1-1024 10.10.10.1-255
Ping | -Pn (no ping), -sP (ping sweep)
Scans | -sS (syn), -sT (TCP), -sF (FIN), -sX (FIN,PUSH, URG), -sM (FIN, ACK), -sU (UDP)
Check for firewall | --badsum (If you recieve a rest or ICMP unreachable, it is likely a firewall)
Numbers instead of machine | -n
Choose ports | -p <start>-<end>
timing | -T <0-5>
Store output | -oN (human readable), -oG (grepable), -oX (XML), -oA (all formats)
Fingerprint | -O
Version scaning | -sV
Fingerprint, version, script, and traceeroute | -A
Reason for result | --reason
