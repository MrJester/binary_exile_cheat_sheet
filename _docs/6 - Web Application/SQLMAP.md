---
title: SQLMap 
category: Web Application
order: 98
---

> **SQL Map Switches**

**Common switches:**

Discover SQL injection | <code> -u </code> 
Spider the site | <code> --crawl </code> 
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
1. <code> Navigate to field with SQL injection (one that has no errors or SQL injection) -> right click on request -> save RAW </code>
2. <code> ./sqlmap.py -l ~/rawrequest.raw   --proxy http://127.0.0.1:8081 </code>


**Start SQLMap based on Burp:**
1) Navigate to request with SQL injection (one that has no errors or SQL injection) -> right click on request -> copy to file
2) ./sqlmap.py -l ~/rawrequest.raw   --proxy http://127.0.0.1:8081



