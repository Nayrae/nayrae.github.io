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