---
title: Recon-NG 
category: Recon
order: 2
---

>Commands 

* See variables
		show options 

* See database structure
		show schema

* Search for a module
		search \<module\>

* Shell Execution 
		Any shell comand
* Reverse Resolve (host identification) 
{% highlight bash %}set NAMESEVER \<DNS Server\> use recon/netblocks-hosts/reverse_resolve
add netblocks <network block that you are interested in>
run" {% endhighlight %}

* Cache Snooping (like software and AV discovery) |{% highlight bash %} use discovery/info_disclosure/cache_snoop
set NAMESEVER <DNS Server>
<option at AV domain to /opt/recon-ng-<version>/data/av_domains.lst>
run"{% endhighlight %}
