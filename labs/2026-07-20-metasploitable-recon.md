# Metasploitable Recon with nmap

**Date:** 2026-07-20 
**Status:** Complete

## Objective
Identify and understand CVE through exploring services and ports on a vulnerable Metasploitable virtual machine.

## Environment
- Attacker: Kali Linux (Host-only IP: 192.168.56.102)
- Target: (Host-only IP: 192.168.56.101)

## Tools Used
- nmap 7.95

## Steps Taken
1. Ran basic nmap scan to identify open ports:
```bash
   nmap 192.168.56.101
```
2. Ran service version scan to fingerprint running software (using -sV to show services and versions):
```bash
   nmap -sV 192.168.56.101
```

## Findings
### Open Ports & Services
| Port | Service | Version | Notes |
|------|---------|---------|-------|
| 21 | FTP | vsftpd 2.3.4 | Known backdoor |
| 22 | SSH | OpenSSH 4.7p1 | Outdated |
| 23 | Telnet | Linux telnetd | Plaintext protocol |
| 80 | HTTP | Apache 2.2.8 | |
| 139/445 | Samba | 3.X-4.X | Vulnerable to usermap_script |
| 1524 | Bindshell | Metasploitable root shell | Open root shell |
| 2121 | FTP | ProFTPD 1.3.1 | Known vulnerabilities |
| 3306 | MySQL | 5.0.51a | |
| 5432 | PostgreSQL | 8.3.0-8.3.7 | |
| 5900 | VNC | Protocol 3.3 | |
| 6667 | IRC | UnrealIRCd | Backdoored version |

## Takeaways
- nmap is the standard first step in any engagement — always run a basic scan 
  first, then -sV for version detection
- Version numbers are critical — knowing vsftpd 2.3.4 vs just "FTP" is the 
  difference between finding a known CVE or not
- Metasploitable2 exposes an unusually high number of services, many with 
  known critical vulnerabilities — in a real environment this attack surface 
  would be catastrophic

## References
- https://nmap.org/
- https://nmap.org/book/man-version-detection.html