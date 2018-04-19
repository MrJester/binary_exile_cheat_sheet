---
title: Internet Gateway and DNS
category: Recon
order: 3
---

> **Whois** 

{% highlight bash %}
$ whois [-h whois_server] name
{% endhighlight %}

> **Registries Whois Databases**

* Europe - [RIPE](http://www.ripe.net/)
* Latin America - [LACNIC](http://www.lacnic.net/en/)
* North America - [ARIN](https://www.arin.net/)
* Asia - [APNIC](http://www.apnic.net/)
* Africa - [AFRINIC](http://www.afrinic.net/)

> **NSLOOKUP** 

{% highlight bash %}
$ nslookup
$ server [server IP or Name]
$ set type=any, MX
#zone transfer
$ ls -d [target domain] 
{% endhighlight %}

> **Host**

{% highlight bash %}
#returns name severs
$ host -t ns megacorpone.com
#returns mail servers
$ host -t mx megacorpone.com
#returns IP for host name
$ host www.megacorpone.com 

#Bruteforce domains using list like https://github.com/danielmiessler/SecLists/tree/master/Discovery/DNS

for name in $(cat list.txt); do
   Host $name.domain.com | grep “has address” |  cut -d “ “ -f1,4
done   
#Reverse lookups

for ip in $(seq 79 92); do
 host X.Y.Z.$ip  | grep “domain” | cut -d” “ -f1,5
done  
{% endhighlight %}

> **DIG** 

{% highlight bash %}
$ dig @[server] [type]
#zone transfer
$ dig @[server] domain -t AXFX  
{% endhighlight %}

> **Useful Resources**

The following resources could be used to traceroute from another host or determine if your are being blocked, or if the host is down:
* [Traceroute.org](www.traceroute.org)
* [Kloth Traceroute](www.kloth.net/services/traceroute.php)
* [tracert.com](www.tracert.com)


