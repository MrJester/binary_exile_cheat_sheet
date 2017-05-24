---
title: Linux
category: Misc
order: 8
---

>**Help and Finding Commands**

Function | Command
------------- | -------------
help | <code> help [cmdlet] -detailed, -example, -ful </code>
Properties and Methods of object or command | <code> [object/command] | gm </code>


>**Inventory, File Search, and Counting**

Function | Command
--------- | -------
Environment Variables | <code>ls env:, ls variable: </code>
Search for Files | <code>ls -r [dir] [string] 2> $null| % {echo $_.fullname} </code>
Search for text in files | <code>ls -r c:\users | % {select-string -path $_ -pattern password} 2>$null </code>


