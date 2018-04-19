---
title: Recon
category: Web Application
order: 1
---

> **WHOIS and DNS**

**DIG and nslookup:**

{% highlight bash %}
#nslookup
nslookup site.org

# DIG
# check Answer section
dig site.org -t any
# any record from 192
dig @[DNSServerIP] site.org -t any
# MX records
dig @[DNSServerIP] site.org -t mx
# Zone Transfer
dig @[DNSServerIP]site.org -t axfr
# PTR reverse lookup
dig -x 192.168.1.2
# Query nameserver's version of BIND
dig @[DNSServerIP] version.bind chaos txt
{% endhighlight %}

**NMAP, DNSRecon, and Metasploit:**

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

> **Open Source Information**

**Google and Bing Hacking**

Google Directives
* site:www.sans.org
* inurl:phpinfo
* intitle:"Admin Login"
* link:sans.org
* ext:xls
* - (negate)

Bing Directives: 
* site:www.sans.org
* inanchor:phpinfo
* intitle:"Admin Login"
* filetype:xls
* not (negate)

**The Harvester**

{% highlight bash %}
theHarvester -d site.org -b all
{% endhighlight %}

**FOCA**

* [FOCA](https://www.elevenpaths.com/labstools/foca/index.html)

**Recon-ng**

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



> **Framework and Configuration**

**User Agent Differences**

{% highlight bash %}
nmap -p80 --script http-useragent-tester.nse [host]
{% endhighlight %}

User Agent Strings:
* [User Agent Strings](http://www.useragentstring.com/index.php?id=19732)

**SSL/TLS Version**

{% highlight bash %}
#openssl
openssl s_client -connect www.site.org:443-ssl2
openssl s_client -connect www.site.org:443 -cipher NULL

#nmap
nmap -p 443 --script=ssl-enum-ciphers www.site.org
{% endhighlight %}

SSLLabs:
* [SSLLabs](www.ssllabs.com)


**Framework and Configuration**

Look for the following:
* known vulnerabilities
* Technologies versions to customize attacks (OS, platform, web server) such as case sensativity
* Supported methods (Put, Delete)
* Default pages 
* Ports and Services

{% highlight bash %}
#NMAP
#Heartbleed
nmap -p 443 --script=ssl-heartbleed.nse www.site.org
#Service Detection
nmap -sV scanme.nmap.org
#Enum - like Nikto
nmap --script=http-enum www.sec542.org
#Methods
nmap --script=http-method www.sec542.org

#nikto set up proxy (if needed)
vim /etc/nikto/config.txt
PROXYHOST=85.28.28.209
PROXYPORT=8080

#nikto
nikto -useproxy -h http://targetsite.com -o results.txt

# netcraft
toolbar.netcraft.com/site_report

#Wappalizer 
https://wappalizer.com
{% endhighlight %}

> **Spidering and Comments**

{% highlight bash %}
#WGET 
wget -r http://www.sec542.org -l 3

#Burp
1. Browse to site while proxied 
2. Click on Spider Tab and verify options
3. Target -> Site Map -> Right click -> spider this host

#ZAP
1. Browse to site while proxied 
2. Right click -> attack -> Spider
3. For javascript: Right click -> attack -> AJAX Spider"

# CeWL - Creates a Custom Word List from Site for usernames and passwords
cewl http://www.sec542.org
{% endhighlight %}

w3af (creates a visual site map):
* [w3af](http://docs.w3af.org/en/latest/basic-ui.html)

>  **Comments and Robots.txt**

When reviewing comments look for:
* useful/sensitive code/links/urls
* disabled functions
* linked servers (contant and application servers)
* passwords
* other interesting information

{% highlight bash %}
#NMAP Comments	
nmap --script=http-comments-displayer www.sec542.org

#Burp Comments, Scripts, References	
1. Right click -> Engagement tools -> Find Comments, Scripts, or References

#NMAP Robots	
nmap --script =http-robots.txt www.sec542.org
{% endhighlight %}

> **Useful Resources**

* [User Agent Strings](http://www.useragentstring.com/index.php?id=19732) 

> **Useful Tools**



