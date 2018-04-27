---
title: Recon-NG 
category: Recon
order: 2
---

> **Basic Commands** 

See variables	| <code>show options </code>
See database structure	| <code>show schema </code>
Search for a module	| <code>search [module] </code>
Shell Execution	| <code>Any shell comand</code>

> **Useful Commands**

**Reverse Resolve (host identification)**
{% highlight bash %}
set NAMESEVER [DNS Server] use recon/netblocks-hosts/reverse_resolve
add netblocks [network block that you are interested]
run
{% endhighlight %}

**Cache Snooping** 
{% highlight bash %}
//(like software and AV discovery) 
use discovery/info_disclosure/cache_snoop
set NAMESEVER [DNS Server] 
[option at AV domain to /opt/recon-ng-[version]/data/av_domains.lst>
run{% endhighlight %}

> **Useful Modules**

{% highlight bash %}
# Usage
# Use [module - recon/[input]-[output]/module]
# show info

#Contacts
recon/contacts-social/dev-diver
recon/contacts-contacts/namechk
recon/companies-contacts/linkedin_auth
recon/companies-contacts/jigsaw
recon/domains-contacts/pgp_search

#Creds
recon/contacts-creds/
recon/creds-creds/
recon/domains-creds/pwnedlist/

#host
recon/hosts-hosts/resolve
recon/domain-hosts/netcraft
recon/hosts-hosts/bin_ip
recon/domain-hosts/shodan_hostname

#Geo
recon/locations-pushpins/picasa
recon/locations-pushpins/shodan
recon/locations-pushpins/twitter
recon/location-pushpins/youtube
{% endhighlight %}
