---
title: Enumerating Users 
category: Scanning
order: 4
---

> **Enumerating Users**

**Passive:**

* Email Addresses
* Blogs
* Newsgroups
* Document Metadata

**Active:**

* /etc/password
* Finger
* Who
* net use
* enum
* user2sid
* sid2user

> **Useful Commands**

**Linux:**

Local Linux | <code> cat /etc/password </code>
Who's logged in | <code> finger, who, w </code>
Remotely Linux | <code> Finger @[targetIP] </code>
NIS | <code> ypcat passwd, ypcat group </code>
LDAP | <code> ldapsearch [criteria] </code>

**Windows:**

NBTSCAN for SMB | <code> nbtscan [IP Range] </code>
Windows null session (<XPsp3 or 2003) |  <code> net use \\targetip "" /u:"" </code>
SMB info - enum | <code> enum -U [targetIP], enum -G [targetIP],  enum -U [or] -G [targetIP] -U [user] -p [password] </code>
SMB info - rpcclient | <code> rpcclient -U "" [IP] <br> Empty Password <br> srvinfo, enumdomusers, getdompwinfo </code>
SMB info - enum4linux | <code> enum4linux -v [IP] </code>
SMB info - NMAP | <code> nmap -p 139,445 --script smb-enum-users [IP] <br> nmap -p 139,445 --script smb-os-discovery.nse [IP] <br> nmap -p 139,445 --script vulns --script-args=unsafe=1 [IP] </code>
Establish an SMB session | <code> net use \\[targetIP] [password] /u:[user] </code>
Request domain/computer part of SID | <code> user2sid \\[targerIP] [machine_name]  </code>
Loop for users  [note: no dashes] | <code> for /L %i in (1000,1,1010) do @sid2user \\[targetIP] [SID without RID] %i  </code

**SMTP:**

Bash | <code>for user in $(cat users.txt); do echo VRFY $user | nc -nv -w 1 192.168.31.215 25 2>/dev/null | grep ^"250"; done </code>
Python | [Python SMPT Enum](https://github.com/BinaryExile/Scripts/tree/master/Recon)

**SNMP:**

note: 1, 2, 2c have no encryption
Find SNMP Machines | <code>nmap -sU --open -p 161 192.168.31.200-254 --open</code>
Bruteforce Community Strings | <code> onesixyone -c community =i ips [MiB Value] </code>
Common Community Strings | [Common Strings](https://github.com/danielmiessler/SecLists/blob/master/Discovery/SNMP/common-snmp-community-strings.txt)
MiB Values | [MiB Values](http://www.oidview.com/mibs/311/md-311-1.html)


> **Useful Resources**

* [User Enumeration](http://pentestmonkey.net/category/tools/user-enumeration)
* [Username Enumeration](https://highon.coffee/blog/penetration-testing-tools-cheat-sheet/#username-enumeration)

