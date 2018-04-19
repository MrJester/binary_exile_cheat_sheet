---
title: Internet Gateway DNS, and Users
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


#Zone Transfer
$ host -t ns domain.com 
$ host -l domain.com ns1.domain.com (obtained in command above)
{% endhighlight %}

> **DIG** 

{% highlight bash %}
$ dig @[server] [type]
#zone transfer
$ dig @[server] domain -t AXFX  
{% endhighlight %}


> **NMAP, DNSRecon, and Metasploit:**

{% highlight bash %}
# NMAP
#Locate NSE Scripts
locate .nse | grep DNS to find scripts
# less the script and look at args and usage
nmap --script=dns-brute www.site.org 
# use DNSRecon list as well /dnsrecon/namelist.txt
nmap -sl 192.168.1.0/24 | grep \)

# domain list is much larger than nmap
dnsrecon.py -d site.org -t brt
dnsrecon.py -r 192.168.1.0/24

#DNSENUM
dnsenum.pl --noreverse -o mydomain.xml example.com

# metasploit
auxiliary/gather/dns-bruteforce
auxiliary/gather/dns-cache-scraper
auxiliary/gather/dns_info
auiliary/gather/dns_reverse_lookup
auiliary/gather/dns_srv_enum
{% endhighlight %}

Note:  Keep a running list of DNS information discovered

> **The Harvester**

{% highlight bash %}
theHarvester -d site.org -b all
{% endhighlight %}

> **FOCA**

* [FOCA](https://www.elevenpaths.com/labstools/foca/index.html)

> **Recon-ng**

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


> **netcraft**

* [netcraft](toolbar.netcraft.com/site_report)

> **Useful Resources**

The following resources could be used to traceroute from another host or determine if your are being blocked, or if the host is down:
* [Traceroute.org](www.traceroute.org)
* [Kloth Traceroute](www.kloth.net/services/traceroute.php)
* [tracert.com](www.tracert.com)


