---
title: Nikto 
category: Web Application
order: 3
---

> **Nikto**

Complete scan | ./nikto.pl -h [target] -p [portNum] -output [filename] -format [csv,htm,txt,xml]

Categories | -T [0-x], (eg., -T 48) <br> 0 - File Upload <br> 1 - Interesting File/Seen in logs  <br>  2 - Misconfiguration / Default File <br> 3 - Information Disclosure <br>  4 - Injection (Xss/Script/HTML) <br>  5 - Remote File Retrieval, in web serve root directory  <br> 6 - Denial of Service, without launching DOS <br> 7 - Remote File Retrival - Server Wide <br> 8 - Command Execution /Remote Shell  <br>  9 - SQL Injection <br> a - Authentication Bypass <br> b - Software Identification <br> x - Exclude this category of checks



