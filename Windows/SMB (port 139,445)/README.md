
### Check for know vulnerabilities and exploits

```
nmap -v -script smb-vuln* -p 139,445 10.10.10.4
```

### Enumerate shares

```
nmap --script smb-enum-shares.nse -p 445 139 10.11.1.31
```

### Check null session

``` 
rpcclient -U "" -N [ip]
-U "" - null session
-N - no password
```

### List shares

```
smbclient -L \\\\[ip]
```