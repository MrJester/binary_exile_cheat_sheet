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
