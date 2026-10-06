# Darshan Hudeda — Security Lab Write-ups

Hands-on attack-and-detect exercises performed in an isolated home lab (Kali Linux + Metasploitable 2, VirtualBox host-only network). Each write-up covers the method used, evidence collected, findings, and how the activity would appear from a defensive/detection standpoint.

## Write-ups

- [01 — vsftpd 2.3.4 Backdoor (CVE-2011-2523)](writeups/01-vsftpd-backdoor.md)
- [02 — SSH Weak Credentials & Legacy Crypto](writeups/02-ssh-weak-credentials.md)
- [03 — UnrealIRCd 3.2.8.1 Backdoor (CVE-2010-2075)](writeups/03-unrealircd-backdoor.md)
- [04 — Reflected XSS in Search (OWASP Juice Shop)](writeups/04-xss-search.md)
- [05 — SQL Injection Authentication Bypass (OWASP Juice Shop)](writeups/05-sqli-login.md)

## Lab Environment

- **Attacker:** Kali Linux (VirtualBox VM)
- **Target:** Metasploitable 2 (VirtualBox VM)
- **Network:** VirtualBox host-only adapter, isolated from external networks
- **Tools:** Nmap, Metasploit Framework, Hydra
