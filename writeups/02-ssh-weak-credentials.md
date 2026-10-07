---
layout: default
---
# SSH Weak Credentials & Legacy Crypto

## Objective
Determine whether the target's SSH service accepts weak, commonly-known credentials.

## Method
Automated attempt first:

hydra -L users.txt -P passwords.txt ssh://192.168.56.102


This initially failed with a key-exchange error, since the target's SSH service only supports obsolete MAC/key algorithms that modern clients reject by default:
[ERROR] could not connect to ssh://192.168.56.102:22 - kex error : no match for method mac algo


After enabling legacy algorithm support in `/etc/ssh/ssh_config` (`HostKeyAlgorithms +ssh-rsa`, `MACs +hmac-md5,hmac-sha1`), Hydra connected but still reported no valid password found. Manual verification was used instead:

ssh msfadmin@192.168.56.102


## Evidence

![SSH manual login as msfadmin](../images/02-ssh-login.png)

## Findings
1. Weak, well-known default credentials (`msfadmin:msfadmin`) grant full shell access.
2. The target forces clients to negotiate obsolete key-exchange/MAC algorithms (pre-2010-era crypto), which also interfered with automated tooling — a secondary weakness in its own right.
3. Automated scanning tools can under-report findings against legacy systems; manual verification caught what Hydra missed.

## Detection Angle
Repeated failed SSH attempts from a single source normally appear in `/var/log/auth.log` as `Failed password` entries, which a brute-force detection rule would catch. Here, Hydra's attempts failed at the protocol/negotiation level rather than the authentication level, meaning this specific run would **not** generate those log entries — a tooling incompatibility that could produce a false sense of security if detection coverage is only validated against the expected log signature.

## Remediation
- Enforce strong, unique credentials; disable or rotate default accounts
- Upgrade SSH configuration to support modern key-exchange and MAC algorithms
- Validate detection coverage against actual tool behavior, not just assumed log output
