# Kali Network Configuration & SSH Access

**Date:** 2026-07-11
**Status:** Complete

## Objective
Fix Kali's NAT adapter to enable internet access, configure apt repositories, 
and set up SSH access from the Windows host for a better workflow.

## Problem
On boot, Kali's eth0 (NAT adapter) was not receiving an IP address automatically, 
leaving the VM without internet access and breaking apt.

## Environment
- Host: HP OMEN 16 (Windows 11)
- Attacker: Kali Linux — eth0 (NAT): 10.0.2.15, eth1 (Host-only): 192.168.56.102
- Target: Metasploitable2 — 192.168.56.101

## Steps Taken

### 1. Diagnose Network Interfaces
```bash
ip a
```
- `eth0` = NAT adapter — UP but no IP assigned
- `eth1` = Host-only adapter — 192.168.56.102 (working)

### 2. Manually Bring Up eth0 (Temporary Fix)
```bash
sudo ip link set eth0 up
sudo ip addr add 10.0.2.15/24 dev eth0
sudo ip route add default via 10.0.2.2
```

### 3. Fix DNS
```bash
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
sudo chattr +i /etc/resolv.conf
```

### 4. Fix APT Repositories
Kali had no apt sources configured:
```bash
echo "deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware" | sudo tee /etc/apt/sources.list
sudo apt update
```

### 5. Make eth0 Persistent via DHCP
Edited network interfaces file so eth0 gets its IP automatically on every boot:
```bash
sudo nano /etc/network/interfaces
```
Added:
```
auto eth0
iface eth0 inet dhcp
```

### 6. Enable SSH
```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

### 7. Connect from Windows Host (WSL)
```bash
ssh ramen@192.168.56.102
```
Added alias for faster access:
```bash
echo "alias kali='ssh ramen@192.168.56.102'" >> ~/.bashrc
source ~/.bashrc
```
Now just type `kali` to connect.

## Verification
- `ip a` after reboot confirms eth0 automatically received 10.0.2.15 via DHCP 
- Pinged Metasploitable2 (192.168.56.102 → 192.168.56.101) successfully 
- SSH access from WSL working via `kali` alias 

## Notes
- eth0 (NAT) provides internet access via VirtualBox's virtual gateway (10.0.2.2)
- eth1 (Host-only) provides isolated lab network access to Metasploitable2
- SSH workflow is preferable to using the VirtualBox window — allows copy/paste from Windows host
- resolv.conf locked with `chattr +i` to prevent DNS settings from resetting on reboot