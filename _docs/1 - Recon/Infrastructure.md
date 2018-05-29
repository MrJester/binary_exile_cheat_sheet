---
title: Infrastructure 
category: Recon
order: 1
---

> **Obtain domains**
	
* [Domain Huter](https://domainhuntergatherer.com/)
* [Expired Domains](https://www.expireddomains.net/)

> **Checking Reputation**
	
* [McAfee](https://trustedsource.org/en/feedback/url?action=checksingle)
* [Fortiguard](https://fortiguard.com/webfilter)
* [Bluecoat and Symantec](http://sitereview.bluecoat.com/)


> **Redirectors**

{% highlight bash %}
iptables -I INPUT -p tcp -m tcp --dport 80 -j ACCEPT
iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination [destinationIP]:80
iptables -t nat -A POSTROUTING -j MASQUERADE
iptables -I FORWARD -j ACCEPT
iptables -P FORWARD ACCEPT
sysctl net.ipv4.ip_forward=1
{% endhighlight %}

* Apache mod_rewrite for specif

> **Domain Fronting**

* [How to Domain Front 1](https://www.optiv.com/blog/escape-and-evasion-egressing-restricted-networks)
* [How to Domain Front 2](https://www.mdsec.co.uk/2017/02/domain-fronting-via-cloudfront-alternate-domains/)
* [How to Domain Front 3](https://blog.cobaltstrike.com/2017/02/06/high-reputation-redirectors-and-domain-fronting/)
* [How to Domain Front 4](https://www.cyberark.com/threat-research-blog/red-team-insights-https-domain-fronting-google-hosts-using-cobalt-strike/)
* [How to Domain Front 5](https://medium.com/@vysec.private/alibaba-cdn-domain-fronting-1c0754fa0142)
* [How to Domain Front - Meterpreter](https://bitrot.sh/post/30-11-2017-domain-fronting-with-meterpreter/)
* [Domain Fronting List](https://github.com/vysec/DomainFrontingLists)
* [Script to find frontable domains and List](https://github.com/peewpw/DomainFrontDiscover)

> **Malleable C2: Cobalt Strike**

* [mudge](https://github.com/rsmudge/Malleable-C2-Profiles)
* [bluescreenofjeff](https://github.com/bluscreenofjeff/Malleable-C2-Randomizer)

> **Agressor Scripts**

* [HarleyQu1nn](https://github.com/harleyQu1nn/AggressorScripts)
*

> **Useful Resources**

* https://github.com/redcanaryco/atomic-red-team/blob/master/Windows/README.md
* https://github.com/Coalfire-Research/Red-Baron
* https://github.com/bluscreenofjeff/Red-Team-Infrastructure-Wiki
* https://bluescreenofjeff.com/2017-01-24-how-to-write-malleable-c2-profilesfor-cobalt-strike/
* https://blog.cobaltstrike.com/2013/02/12/a-vision-for-distributed-red-team-operations/
* https://blog.cobaltstrike.com/2014/09/09/infrastructure-for-ongoing-red-team-operations/
* Advanced Threat Tactics (2 of 9): Infrastructure - Raphael Mudge
* https://www.youtube.com/watch?v=3gBJOJb8Oi0
* https://cybersyndicates.com/2016/11/top-red-team-tips/

