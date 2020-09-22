
### Initail recon
```
nmap -A -sV <ip> -p- -vv
```


### Find generic vulns

```
sudon map -sV -p- --script=nfs-* <IP>
```


### Full UDP scan

```
nmap -sU -p- <IP>
```
