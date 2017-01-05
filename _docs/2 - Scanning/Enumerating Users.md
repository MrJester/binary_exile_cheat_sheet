---
title: Enumerating Users 
category: Scanning
order: 4
---

> **Enumerating Users**

Passive:
* Email Addresses
* Blogs
* Newsgroups
* Document Metadata

Active:
* /etc/password
* Finger
* Who
* net use
* enum
* user2sid
* sid2user

> **Useful Commands**

Linux:
Local Linux | cat /etc/password
Who's logged in | finger, who, w
Remotely Linux | Finger @[targetIP]
NIS | ypcat passwd, ypcat group
LDAP | ldapsearch [criteria]

Windows:
Windows null session | net use \\targetip "" /u:""
SMB info | enum -U [targetIP], enum -G [targetIP],  enum -U [or] -G [targetIP] -U [user] -p [password]
Establish an SMB session | net use \\[targetIP] [password] /u:[user]
Request domain/computer part of SID | user2sid \\[targerIP] [machine_name]
Loop for users  [note: no dashes] | for /L %i in (1000,1,1010) do @sid2user \\[targetIP] [SID without RID] %i

> **Useful Resources**

[User Enumeration](http://pentestmonkey.net/category/tools/user-enumeration)

