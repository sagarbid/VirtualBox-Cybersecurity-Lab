# VirtualBox Cybersecurity Lab

A hands-on virtual lab environment built with **VirtualBox**, featuring **Windows**, **Ubuntu**, and **Kali Linux** VMs networked together on an isolated host-only subnet. This is the base environment every other project in this portfolio runs on top of.

## 🎯 Overview

This lab simulates a small, isolated enterprise network for safe experimentation — attacker machine, vulnerable Linux target, and Windows target, all reachable from each other but never from the host's real LAN.

**Key goals:**
- Practice network configuration and segmentation with host-only VirtualBox networking
- Run reconnaissance, vulnerability scanning, and controlled exploitation
- Generate real authentication/attack telemetry for SIEM detection projects
- Build skills relevant to SOC Analyst roles: log analysis, host hardening, network segmentation

**VM roles:**

| VM | Role | Used by |
|---|---|---|
| **Kali Linux** | Attacker machine (Nmap, Hydra, Metasploit, Wireshark) | Every other project in this portfolio |
| **Ubuntu Server** | Vulnerable target (Apache, OpenSSH) | SSH brute-force / log analysis exercises |
| **Windows** | Client/RDP target (Event Log auditing enabled) | [RDP brute-force → Splunk detection](https://github.com/sagarbid/rdp-bruteforce-splunk-detection), Wazuh/Splunk SIEM projects |

## 🏗️ Lab Topology

```
┌─────────────────────────────────────────────────────────┐
│                  Host-Only Network (vboxnet0)             │
│                     192.168.56.0/24                       │
│                                                             │
│   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐ │
│   │ Kali Linux    │   │ Ubuntu        │   │ Windows      │ │
│   │ (attacker)    │   │ (target)      │   │ (target)     │ │
│   └──────┬───────┘   └──────┬───────┘   └──────┬───────┘ │
│          │                   │                   │         │
└──────────┼───────────────────┼───────────────────┼─────────┘
           │                   │                   │
      NAT (internet)      NAT (internet)      NAT (internet)
```

Full networking setup and verification steps: [`docs/networking.md`](docs/networking.md)

## 🛠️ Technologies & Requirements

| Component | Details |
|---|---|
| **Hypervisor** | VirtualBox 7.x |
| **Host OS** | macOS / Windows / Linux, 16 GB+ RAM |
| **Kali** | 2025.x ISO from kali.org |
| **Ubuntu** | 24.04 LTS Server ISO |
| **Windows** | 10/11 Evaluation ISO from Microsoft |

Full host requirements and ISO sources: [`setup/host-requirements.md`](setup/host-requirements.md)

## 🚀 Setup Instructions

1. **Host preparation** — [`setup/host-requirements.md`](setup/host-requirements.md)
2. **Create the three VMs:**
   - Kali — [`setup/kali-vm-specs.md`](setup/kali-vm-specs.md)
   - Ubuntu — [`setup/ubuntu-vm-specs.md`](setup/ubuntu-vm-specs.md)
   - Windows — [`setup/windows-vm-specs.md`](setup/windows-vm-specs.md)
3. **Networking** — [`docs/networking.md`](docs/networking.md)

> ⚠️ Download ISOs fresh from official sources. Never commit ISO files to this repo.

## 📸 Evidence

This repo currently documents the build process and configuration in full; VM Manager and connectivity-test screenshots from the live lab aren't captured yet. They'll be added to a `screenshots/` folder once taken — until then, the attack/detection projects that run on top of this lab ([Wazuh homelab](https://github.com/sagarbid/wazuh-homelab-soc), [RDP brute-force detection](https://github.com/sagarbid/rdp-bruteforce-splunk-detection)) carry the actual screenshot evidence of this environment in use.

## 💡 Lessons Learned

- **Host-only networking is the right call over bridged for this kind of lab** — it keeps every deliberately-vulnerable VM off the real LAN entirely, at the cost of needing NAT as a second adapter just for internet access during setup.
- **RAM allocation across three running VMs adds up fast** — 4+2+4 GB plus host OS overhead means 16 GB is a true minimum, not a comfortable one; running all three simultaneously on a 16 GB host leaves very little headroom for anything else.
- **Auditing has to be turned on deliberately** — Windows doesn't log failed RDP/logon attempts at the level needed for the SIEM projects by default; `auditpol` has to be configured before any attack simulation, or the telemetry those other projects depend on simply won't exist.

## 🔧 What I'd Improve

- **Capture and add the actual screenshots** (VM Manager overview, successful ping tests between all three VMs) — this is the most overdue fix, flagged above.
- **Script the VM creation itself** (VBoxManage CLI or Vagrant) instead of manual click-through steps — right now rebuilding this lab from scratch means re-reading three setup docs and clicking through VirtualBox's UI each time, which doesn't scale if a VM needs to be rebuilt.
- **Document a snapshot strategy.** None of the setup docs mention taking a clean snapshot after initial build — without one, resetting a VM after an attack exercise means a full reinstall instead of a one-click revert.
- **Add firewall rules documentation** for each VM's host-only adapter — right now "isolated" relies on the host-only network not being bridged, with no additional per-VM firewall hardening documented.

## 👤 Author

**Sagar Bidari**
CompTIA Security+ CE | Monash University Cybersecurity Bootcamp Graduate
🌐 [bidarisagar.com](https://bidarisagar.com) | 💼 [LinkedIn](https://linkedin.com/in/sagarbidari)
