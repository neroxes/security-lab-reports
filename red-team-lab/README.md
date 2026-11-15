# Red Team Lab: Recon, Exploitation and Remediation

Three connected lab projects: building an isolated attack lab, enumerating a vulnerable target, then exploiting it and documenting the fixes.
**All testing was done against intentionally vulnerable VMs that I own, on an isolated host-only network.**

## Reports
| # | Report | Summary |
|---|---|---|
| 1 | [Lab setup](project1-lab-setup.pdf) | VirtualBox lab with Kali Linux, Ubuntu Server and Metasploitable 2 on 192.168.56.0/24, with connectivity and isolation checks |
| 2 | [Network discovery and enumeration](project2-network-discovery-enumeration.pdf) | Nmap, enum4linux, DIRB, WhatWeb and smbclient against Metasploitable 2. 30+ open ports, web apps, SMB null session, weak password policy, risk ratings and remediation |
| 3 | [Exploitation and post-exploitation](project3-exploitation-post-exploitation.pdf) | Exploited the unauthenticated bindshell and the Samba usermap_script flaw, examined the system and database credentials, and covered cleanup. Includes mitigation and monitoring advice |

## Key risks found on the target
- vsftpd 2.3.4 backdoor (CVE-2011-2523)
- Unauthenticated root bindshell on port 1524
- Samba 3.0.20 usermap_script remote code execution
- SMB null sessions and anonymous FTP
- Weak password policy and MD5 password hashes in a web app database

## Tools
Kali Linux, Nmap, Metasploit, enum4linux, DIRB, WhatWeb, smbclient, Hydra, VirtualBox.
