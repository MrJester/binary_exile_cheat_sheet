---
title: TCP Dump 
category: Scanning
order: 1
---

> **TCP Dump** 

Example Command	| {% highlight bash %}tcpdump -nnX tcp and port 80 and host 10.10.10.10 {% endhighlight %}
Protocols | ether, ip, ip6, arp, rarp, tcp, udp
Type | host, net, port, portrange
Direction | src, dst
Use numbers for machines and ports | -nn
Write to file | -w
print out in ascII or hex | -A -X
Choose interface | -i [interface]
Full packets | -s 
Network filter | Net 10.10.10


