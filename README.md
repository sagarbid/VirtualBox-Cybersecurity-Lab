# VirtualBox Cybersecurity Lab

A hands-on virtual lab environment built with **VirtualBox** featuring **Windows**, **Ubuntu**, and **Kali Linux** VMs. Designed for cybersecurity training, network testing, and ethical hacking practice. [web:16][web:18]

## 🎯 Overview

This lab simulates a small enterprise network with isolated VMs for safe experimentation. 

**Key goals:**
- Practice network configuration and segmentation.
- Run reconnaissance, vulnerability scanning, and basic exploitation.
- Test incident response workflows.
- Demonstrate skills relevant to SOC Analyst roles (log analysis, host hardening). [web:18][web:19]

**VM Roles:**
- **Kali Linux**: Attacker machine for pentesting tools (Nmap, Metasploit).
- **Ubuntu**: Vulnerable target (web server, SSH).
- **Windows**: Client workstation (endpoint with basic services).

## 🏗️ Lab Topology


**Network Details:**
- **Gateway**: 192.168.56.1 (VirtualBox host-only).
- All VMs have **dual adapters**: NAT (internet) + Host-Only (inter-VM communication). [web:21][web:27]

![Lab Overview](docs/screenshots/01-vm-manager.png)

## 🛠️ Technologies & Requirements

| Component       | Details                          |
|-----------------|----------------------------------|
| **Hypervisor** | VirtualBox 7.x (latest)         |
| **Host OS**    | macOS/Windows/Linux (16GB+ RAM) |
| **Kali**       | 2025.x ISO from kali.org        |
| **Ubuntu**     | 24.04 LTS Server ISO            |
| **Windows**    | 10/11 Evaluation ISO from MS    |

**Minimum specs**: 16GB host RAM, 4 cores allocated across VMs. [web:7][web:21]

## 🚀 Setup Instructions

### 1. Host Preparation (`setup/host-requirements.md`)

### 2. Create VMs (see `setup/` folder)
- **Kali** (`setup/kali-vm-specs.md`): 4GB RAM, 50GB VDI, Adapter1=NAT, Adapter2=Host-Only.
- **Ubuntu** (`setup/ubuntu-vm-specs.md`): 2GB RAM, 30GB VDI. Install: `sudo apt install apache2 openssh-server`.
- **Windows** (`setup/windows-vm-specs.md`): 4GB RAM, 50GB VDI. Enable RDP/File Sharing.

**⚠️ Download ISOs fresh from official sources. Never commit them.** [web:7]

### 3. Networking (`docs/networking.md`)

**Test connectivity:**

![Ping Test](docs/screenshots/02-ping-success.png)
