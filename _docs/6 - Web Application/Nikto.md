---
title: Nikto 
category: Web Application
order: 1
---

> **Nikto**

Complete scan | ./nikto.pl -h [target] -p [portNum] -output [filename] -format [csv,htm,txt,xml]

Categories | -T [0-x], (eg., -T 48) <cr> 0 - File Upload <cr> 1 - Interesting File/Seen in logs  <cr>  2 - Misconfiguration / Default File <cr> 3 - Information Disclosure <cr>  4 - Injection (Xss/Script/HTML) <cr>  5 - Remote File Retrieval, in web serve root directory  <cr> 6 - Denial of Service, without launching DOS <cr> 7 - Remote File Retrival - Server Wide <cr> 8 - Command Execution /Remote Shell  <cr>  9 - SQL Injection <cr> a - Authentication Bypass  <cr> b - Software Identification <cr> x - Exclude this category of checks



