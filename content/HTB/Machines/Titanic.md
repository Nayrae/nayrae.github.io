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
after playing a bit with the main website we find that the **Book Now** option, once every camp is filled, gives us a json ticket with a get request, if we modify that  request we find it to be vulnerable to **path traversal**
![[Pasted image 20251228234958.png]]
![[Pasted image 20251228233918.png]]
But nothing else was found.
However, we find the **dev** subdomain to use **Gitea** 
![[Pasted image 20251229000314.png]]
In the **Explore** section we find 2 repos:
- flask-app (what the main website is using)
- docker-config (mysql and gitea infos can be found here)
![[Pasted image 20251229000346.png]]
