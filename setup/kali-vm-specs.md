# Kali Linux VM — Attacker Machine

## VirtualBox settings

| Setting | Value |
|---|---|
| Type/Version | Linux / Debian (64-bit) |
| RAM | 4096 MB |
| CPUs | 2 |
| Disk | 50 GB, VDI, dynamically allocated |
| Adapter 1 | NAT (internet access for `apt update`, tool downloads) |
| Adapter 2 | Host-Only Adapter (`vboxnet0`) — this is how Kali reaches the Ubuntu and Windows VMs |

## Build steps

1. Create the VM in VirtualBox with the specs above, attach the Kali ISO, and install normally.
2. After install, confirm both adapters are up:
   ```bash
   ip a
   # eth0 (NAT)       — should get a DHCP address, usually 10.0.2.x
   # eth1 (Host-Only) — should get an address on 192.168.56.0/24
   ```
3. Update and install the core toolset used across this lab's other projects:
   ```bash
   sudo apt update && sudo apt full-upgrade -y
   sudo apt install -y nmap hydra metasploit-framework wireshark
   ```
4. Confirm reachability to the other VMs over the host-only network:
   ```bash
   ping -c 3 192.168.56.<ubuntu-ip>
   ping -c 3 192.168.56.<windows-ip>
   ```

Kali is the attacker role for every exercise run in this lab (the Nmap scanner, the Hashcat cracking project, the RDP brute-force project, and the Wazuh homelab all launch from this VM).
