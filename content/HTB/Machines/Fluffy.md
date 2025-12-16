---
{"publish":true,"created":"2025-12-16T15:14:02.227+01:00","modified":"2025-12-16T15:15:10.028+01:00","tags":["HTB","Windows","Easy"],"cssclasses":""}
---

# Reconnaissance

We're give the credentials `j.fleischman / J0elTHEM4n1990!`, however the first step is a nmap scan to see what services are enabled, and maybe we're able to find some other useful information
![[Pasted image 20251216152400.png]]
We're able to obtain lots of interesting informations: 
- kerberos, ldap, smb (port 445), ldaps
- the the FQDN is `dc01.fluffy.htb`
	- and that's what we have to add to our `/etc/hosts` file, after `fluffy.htb`

If we enumerate smb shares with the account we received, we can see that our account has `read-write` access to the `IT` share,  we download the files, they all seem interesting
![[Pasted image 20251216181254.png]]
In fact `Upgrade_Notice.pdf` reveals to be a Notice made for the IT Department, which tells the administrators all the vulnerabilities found in the system
![[Pasted image 20251216181728.png]]After searching we find `CVE-2025-24071` to be the right one, with this exploit we first create a zip file which, when opened by the victim, will send to the attacker ip running `responder` the `NTLM hash` of the user
![[Pasted image 20251216203438.png]]