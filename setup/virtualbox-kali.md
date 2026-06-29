# VirtualBox Kali Linux Setup

**Date:** 2026-06-29  
**Status:** Complete

## Objective
Set up a Kali Linux VM in VirtualBox as the attacker machine for the home cyber lab.

## Environment
- Host: HP OMEN 16 (Windows 11, 32GB RAM, RTX 5070)
- Hypervisor: VirtualBox 7.x
- Guest OS: Kali Linux 2.6 (64-bit)

## VM Configuration
| Setting | Value |
|---|---|
| Base Memory | 8192 MB |
| CPUs | 4 |
| Video Memory | 128 MB |
| Storage | 30 GB (SATA, VDI) |
| Adapter 1 | NAT (internet access) |
| Adapter 2 | Host-only Adapter (lab network) |

## Steps Taken
1. Installed Kali Linux in VirtualBox
2. Discarded saved state to allow settings changes
3. Increased RAM from 4GB to 8GB
4. Increased CPUs from 2 to 4
5. Increased video memory from 16MB to 128MB
6. Removed floppy from boot order
7. Added Host-only Adapter as Adapter 2 for internal lab networking
8. Took snapshot named `clean-install`

## Notes
- NAT on Adapter 1 allows Kali to reach the internet for updates/tool installs
- Host-only on Adapter 2 allows Kali to communicate with other lab VMs (e.g. Metasploitable 2)
- Snapshot `clean-install` taken before first boot — restore point if anything breaks

## Next Steps
- Download and configure Metasploitable 2 as target machine
- Verify Kali ↔ Metasploitable connectivity over Host-only network