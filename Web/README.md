## Enumeration

### Gobuster
```
gobuster dir -u https://10.11.1.35 -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt -t 50 -k -o gobuster

gobuster dir -u http://10.11.1.72 -w /usr/share/SecLists/Discovery/Web-Content/common.txt -s '200,204,301,302,307,403,500' -e

```
### Dirsearch
https://github.com/maurosoria/dirsearch
```
python3 dirsearch.py -u <URL> -e <EXTENSION>
```

### Nikto

```
nikto -h IP_ADDR
```