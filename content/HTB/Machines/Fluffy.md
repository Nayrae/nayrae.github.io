---
{"publish":true,"created":"2025-12-16T15:14:02.227+01:00","modified":"2025-12-16T15:15:10.028+01:00","tags":["HTB","Windows","Easy"],"cssclasses":""}
---
<<<<<<< HEAD
# Reconnaissance

We're give the credentials `j.fleischman / J0elTHEM4n1990!`, however the first step is a nmap scan to see what services are enabled, and maybe we're able to find some other useful information
![[Pasted image 20251216152400.png]]
We're able to obtain lots of interesting informations: 
- kerberos, ldap, smb (port 445), ldaps
- the the FQDN is `dc01.fluffy.htb`
	- and that's what we have to add to our `/etc/hosts` file, after `fluffy.htb`
=======

>>>>>>> origin/v4
