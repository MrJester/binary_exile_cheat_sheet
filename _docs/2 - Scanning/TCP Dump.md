---
title: TCP Dump 
category: Scanning
order: 99
---

> **TCP Dump** 

Example Command	| <code>tcpdump -nnX tcp and port 80 and host 10.10.10.10 </code>
Protocols | ether, ip, ip6, arp, rarp, tcp, udp
Type | host, net, port, portrange
Direction | src, dst
Use numbers for machines and ports | -nn
Write to file | -w
print out in ascII or hex | -A -X
Choose interface | -i [interface]
Full packets | -s 
Network filter | Net 10.10.10


