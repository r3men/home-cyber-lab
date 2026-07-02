# Metasploitable2 Setup

**Date:** 2026-07-02
**Status:** Complete

## Objective
Setup a Metasploitable2 VM in VirtualBox as the target machine for attacks made by the attacker machine.

## Environment
- Host: HP OMEN 16 (Windows 11)
- Hypervisor: VirtualBox 7.x
- Guest OS: Ubuntu 8.04 (32-bit) — Metasploitable2

## VM Configuration
| Setting | Value |
|---|---|
| Base Memory | 512 MB |
| CPUs | 1 |
| Storage | 8 GB (existing .vmdk) |
| Adapter 1 | Host-only (VirtualBox Host-Only Ethernet Adapter) |

## Steps Taken
1. Download the zip file for Metasploitable2 via SourceForge.
2. Extract the zip file to retrieve the .vmdk file, which will serves as the physical storage for the target VM.
3. Create a new virtual machine in VirtualBox and adjust settings (512MB RAM, 1 CPU, Linux)
4. For the hard disk, point to the virtual hard disk file from the extracted zip.
5. Adjust network settings for the VM by setting Adapter 1 to "host-only" rather than NAT, ensuring the machine can only be targeted in a safe, controlled, private internal network.

## Notes
- Metasploitable2 uses Host-only networking only (no NAT) because it is intentionally vulnerable, as
  exposing it to the internet would be a security risk.
- Default credentials: msfadmin / msfadmin
- NAT is reserved for machines that need internet access (e.g. Kali Linux Adapter 1).

## Next Steps
- Boot Metasploitable2 and retrieve its Host-only IP via `ifconfig`
- Verify connectivity from Kali via `ping`
- Conduct initial nmap scan from Kali to enumerate open ports and services