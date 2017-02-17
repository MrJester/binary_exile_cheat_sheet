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

Windows null session |  <code> net use \\targetip "" /u:"" </code>
SMB info | <code> enum -U [targetIP], enum -G [targetIP],  enum -U [or] -G [targetIP] -U [user] -p [password] </code>
Establish an SMB session | <code>net use \\[targetIP] [password] /u:[user] </code>
Request domain/computer part of SID | <code> user2sid \\[targerIP] [machine_name]  </code>
Loop for users  [note: no dashes] | <code> for /L %i in (1000,1,1010) do @sid2user \\[targetIP] [SID without RID] %i  </code>

> **Useful Resources**

[User Enumeration](http://pentestmonkey.net/category/tools/user-enumeration)

