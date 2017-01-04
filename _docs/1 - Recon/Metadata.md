---
title: Metadata 
category: Recon
order: 3
---

> **Exiftool** 

{% highlight bash %}
$ exiftool [filename]
{% endhighlight %}

> **Strings** 

{% highlight bash %}
# ASCII
$ strings -td -n [wordlength] filename
# Unicode
$ strings -td -el -n [wordlength] filename
# Websites and Folders
$ strings -n 8 [filename] | grep /
$ strings -n 8 [filename] | grep \\
# Email Addresses 
$ strings -n 3 [filename] | grep @
$ strings -n 3 [filename] | grep '@'
{% endhighlight %}

> **Useful Tools**

* FOCA
* Metadata Extraction Tool by National Library of New Zealand
* Sysinternals strings


Topic		|Command
-----		| -------
Example Command	| tcpdump -nnX tcp and port 80 and host 10.10.10.10
Protocols 	| ether, ip, ip6, arp, rarp, tcp, udp
Type 		| host, net, port, portrange
