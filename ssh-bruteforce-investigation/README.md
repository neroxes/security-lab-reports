# SSH Brute-Force Attack: Detection, Analysis and Response

A controlled attack on my own Ubuntu server, followed by a log investigation of what the attack looked like and whether fail2ban stopped it.

**Report:** [ssh-bruteforce-investigation-report.pdf](ssh-bruteforce-investigation-report.pdf) (8 pages)

## Lab setup
- Target: Ubuntu 24.04 Server (192.168.56.4) running OpenSSH and fail2ban with the default sshd jail
- Attacker: Kali Linux (192.168.56.5) using Hydra 9.5
- Isolated VirtualBox host-only network. Logs reviewed: `/var/log/auth.log` and `/var/log/fail2ban.log`

## What happened
1. **Two noisy waves** with many usernames and passwords: fail2ban detected both and banned the attacker within seconds
2. **One targeted attempt** with the correct password as the first guess: the login succeeded immediately, no failures were logged, and **fail2ban never triggered**

## Key findings
- fail2ban works against loud attacks but only counts failed logins, so it cannot flag a successful login from an unusual source
- The real cause was a weak password (the password matched the username)
- A successful attacker login looks the same as a normal one in the raw log, apart from the source IP

## Recommendations
Use SSH keys and disable password login, set `PermitRootLogin no`, add MFA, forward logs to a central collector with an alert on `Accepted password` from IPs outside an allowlist, and lengthen bans for repeat offenders.

## Commands used
`grep`, `sort | uniq -c`, `fail2ban-client status sshd` and `lnav` on the log files. The full list is in the report.
