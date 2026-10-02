# Windows VM — Client Workstation / RDP Target

## VirtualBox settings

| Setting | Value |
|---|---|
| Type/Version | Windows 10/11 (64-bit) |
| RAM | 4096 MB |
| CPUs | 2 |
| Disk | 50 GB, VDI, dynamically allocated |
| Adapter 1 | NAT (Windows Update, tool downloads) |
| Adapter 2 | Host-Only Adapter (`vboxnet0`) |

## Build steps

1. Install Windows from the Microsoft Evaluation Center ISO.
2. Confirm network adapters in an elevated PowerShell prompt:
   ```powershell
   ipconfig /all
   ```
3. Enable the services this VM exposes for the lab exercises:
   ```powershell
   # Enable Remote Desktop
   Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 0
   Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

   # Enable failed/successful logon auditing (needed for Event IDs 4624/4625)
   auditpol /set /subcategory:"Logon" /success:enable /failure:enable
   ```
4. Create a disposable local test account for any exercise that needs a target account — never reuse a real credential:
   ```powershell
   net user labtest P@ssw0rdTest123 /add
   ```

This VM is the RDP target used in the [RDP brute-force → Splunk detection](https://github.com/sagarbid/rdp-bruteforce-splunk-detection) project, and the Windows event-log source forwarded to Splunk/Wazuh in the SIEM-focused projects.

> **Security note:** Network Level Authentication (NLA) is enabled by default and should stay enabled except when a specific exercise explicitly requires disabling it for compatibility (documented in that exercise's own README, not here).
