# Host Preparation

Before creating any VM, confirm the host machine can actually run this lab.

## Minimum host specs

| Resource | Minimum | Recommended |
|---|---|---|
| RAM | 16 GB | 32 GB |
| CPU cores | 4 (virtualized across all VMs) | 6+ |
| Free disk | 130 GB (50+30+50 GB for the three VMs) | 200 GB (room for snapshots) |
| Virtualization support | Intel VT-x / AMD-V enabled in BIOS/UEFI | — |

## Software

1. **Install VirtualBox 7.x** from [virtualbox.org](https://www.virtualbox.org/wiki/Downloads) — the Extension Pack isn't required for this lab (no USB passthrough or RDP display needed).
2. **Enable virtualization in firmware** if VM creation fails with a "VT-x is not available" error — this is a BIOS/UEFI setting, not a VirtualBox one.
3. **Disable conflicting hypervisors** on Windows hosts — Hyper-V and VirtualBox can't both claim VT-x at once. Either disable Hyper-V (`bcdedit /set hypervisorlaunchtype off`, then reboot) or run VirtualBox in Hyper-V-compatible mode (slower, nested virtualization).

## ISOs needed

Download these fresh from official sources — **never commit ISO files to this repo**:

| VM | Source |
|---|---|
| Kali Linux | [kali.org/get-kali](https://www.kali.org/get-kali/) |
| Ubuntu Server 24.04 LTS | [ubuntu.com/download/server](https://ubuntu.com/download/server) |
| Windows 10/11 Evaluation | [microsoft.com/evalcenter](https://www.microsoft.com/en-us/evalcenter/) |

Once the host meets these requirements, move on to creating the three VMs — see `kali-vm-specs.md`, `ubuntu-vm-specs.md`, and `windows-vm-specs.md` in this folder.
