---
title: Linux
category: Misc
order: 90
---

>**Screen**

Function | Command
------------- | -------------
Create Screen: | screen 
Create Window | Ctrl-a c
Switch Window: | Ctrl-a n
Split Screen: | <code> Ctrl-a S or | </code>
Switch Split Window:| Ctrl-a tab
Detatch from screen: | Ctrl-a d
Attach to screen: | screen -r  31844.pts-0.name
Logging Screen Output | Ctrl-a H
Kill current screen: | Ctrl-a K
Kill a screen: | screen -X -S 12937 quit 

>**Files and Folders**

Function | Command
--------- | -------
Mount Windows CIFS/SMB Share | <code> mount -t cifs -o username=user,password=pass,domain=blah //192.168.1.X/share-name /mnt/cifs </code>
Mount NFS share | <code> mount 192.168.1.1:/vol/share /mnt/nfs </code>
GUI for SMB shares | apt-get install smb4k -y


