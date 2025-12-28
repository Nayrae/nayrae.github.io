---
publish: true
tags:
  - HTB
  - Easy
  - Linux
---
# Reconnaissance

From a nmap scan we find **ssh** and **http** services (port 22 and 80) open.
![[Pasted image 20251228223424.png]]
After trying to reach the site we find the domain **titanic.htb** and set it in out '/etc/hosts', before looking at the website we start a background subdomain fuzz which finds us **dev** subdomain
![[Pasted image 20251228231306.png]]
after playing a bit with the main website we 
![[Pasted image 20251228233918.png]]