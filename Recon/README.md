
### Initail recon
```
Nmap -T4 -sV -Pn -p- IP
```


### Find generic vulns for a specific service (this example nfs)

```
sudon map -sV --script=nfs-* <IP>
```


### Full UDP scan

```
nmap -sU -p- <IP>
```
