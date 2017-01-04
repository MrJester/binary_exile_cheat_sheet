---
title: Recon-NG 
category: Recon
order: 2
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

> **Useful Commands**

Reverse Resolve (host identification) 

{% highlight bash %}
set NAMESEVER <DNS Server> use recon/netblocks-hosts/reverse_resolve
add netblocks <network block that you are interested in>
run
{% endhighlight %}

Cache Snooping (like software and AV discovery) 

{% highlight bash %}
use discovery/info_disclosure/cache_snoop
set NAMESEVER <DNS Server> 
<option at AV domain to /opt/recon-ng-<version>/data/av_domains.lst>
run{% endhighlight %}
