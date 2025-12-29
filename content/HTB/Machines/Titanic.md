---
publish: false
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
![[Pasted image 20251229140421.png]]

But nothing else was found.
However, we find the **dev** subdomain to use **Gitea** 
![[Pasted image 20251229000314.png]]
In the **Explore** section we find 2 repos:
- flask-app (what the main website is using)
- docker-config (mysql and gitea infos can be found here)
![[Pasted image 20251229000346.png]]

In docker-config/mysql we get the admin credentials for the database
![[Pasted image 20251229000847.png]]
In the **gitea** folder we find the volume and ports where the service is running
![[Pasted image 20251229001310.png]]
After looking online where should gitea db file should be located and a few tries we find **gitea.db** ![[Pasted image 20251229002306.png]]
We download the file and open it with sqlite3

```
htb/machines/Titanic ❯ sqlite3 gitea.db                                                                                           
SQLite version 3.51.1 2025-11-28 17:28:25
Enter ".help" for usage hints.
sqlite> .tables
󰣇 htb/machines/Titanic ❯ sqlite3 gitea.db                                                                                                                                                                       
SQLite version 3.51.1 2025-11-28 17:28:25
Enter ".help" for usage hints.
sqlite> .tables
access                     oauth2_grant             
access_token               org_user                 
action                     package                  
action_artifact            package_blob             
action_run                 package_blob_upload      
action_run_index           package_cleanup_rule     
action_run_job             package_file             
action_runner              package_property         
action_runner_token        package_version          
action_schedule            project                  
action_schedule_spec       project_board            
action_task                project_issue            
action_task_output         protected_branch         
action_task_step           protected_tag            
action_tasks_version       public_key               
action_variable            pull_auto_merge          
app_state                  pull_request             
attachment                 push_mirror              
auth_token                 reaction                 
badge                      release                  
branch                     renamed_branch           
collaboration              repo_archiver            
comment                    repo_indexer_status      
commit_status              repo_redirect            
commit_status_index        repo_topic               
commit_status_summary      repo_transfer            
dbfs_data                  repo_unit                
dbfs_meta                  repository               
deploy_key                 review                   
email_address              review_state             
email_hash                 secret                   
external_login_user        session                  
follow                     star                     
gpg_key                    stopwatch                
gpg_key_import             system_setting           
hook_task                  task                     
issue                      team                     
issue_assignees            team_invite              
issue_content_history      team_repo                
issue_dependency           team_unit                
issue_index                team_user                
issue_label                topic                    
issue_user                 tracked_time             
issue_watch                two_factor               
label                      upload                   
language_stat              user                     
lfs_lock                   user_badge               
lfs_meta_object            user_blocking            
login_source               user_open_id             
milestone                  user_redirect            
mirror                     user_setting             
notice                     version                  
notification               watch                    
oauth2_application         webauthn_credential      
oauth2_authorization_code  webhook                  
sqlite> select * from user;
1|administrator|administrator||root@titanic.htb|0|enabled<SNIP>|pbkdf2$50000$50|0|0|0||0|||70a5bd0c1a5d23caa49030172cdcabdc|2d149e5fbd1b20cf31db3e3c6a28fc9b|en-US||1722595379|1722597477|1722597477|0|-1|1|1|0|0|0|1|0|2e1e70639ac6b0eecbdab4a3d19e0f44|root@titanic.htb|0|0|0|0|0|0|0|0|0||gitea-auto|0
2|developer|developer||developer@titanic.htb|0|enabled|e53<SNIP>ef30bf1682619263444ea594cfb56|pbkdf2$50000$50|0|0|0||0|||0ce6f07fc9b557bc070fa7bef76a0d15|8bf3e3452b78544f8bee9400d6936d34|en-US||1722595646|1722603397|1722603397|0|-1|1|0|0|0|0|1|0|<SNIP>|developer@titanic.htb|0|0|0|0|2|0|0|0|0||gitea-auto|0
sqlite> 
```


![[Pasted image 20251229141536.png]]