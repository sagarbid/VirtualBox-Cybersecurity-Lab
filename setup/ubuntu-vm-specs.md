# Ubuntu Server VM — Vulnerable Target

## VirtualBox settings

| Setting | Value |
|---|---|
| Type/Version | Linux / Ubuntu (64-bit) |
| RAM | 2048 MB |
| CPUs | 1 |
| Disk | 30 GB, VDI, dynamically allocated |
| Adapter 1 | NAT (package installs) |
| Adapter 2 | Host-Only Adapter (`vboxnet0`) — reachable from Kali and Windows |

## Build steps

1. Install Ubuntu Server 24.04 LTS from the official ISO. During install, enable OpenSSH server when prompted (or install it after).
2. Confirm network adapters:
   ```bash
   ip a
   ```
3. Install the services this VM exposes for the lab exercises:
   ```bash
   sudo apt update
   sudo apt install -y apache2 openssh-server
   sudo systemctl enable --now apache2 ssh
   ```
4. Confirm both services are listening:
   ```bash
   sudo ss -tlnp | grep -E ':22|:80'
   ```

This VM plays the "vulnerable target" role — its SSH service is the target for the brute-force exercises, and Apache is the target for the web-log analysis exercises referenced elsewhere in this lab series.

> **Security note:** this VM is deliberately under-hardened for the exercises run against it (weak test accounts, default configs). Never expose this VM's host-only or NAT adapter to a real network — it should only ever be reachable from other VMs inside this same isolated lab.
