# Lab Networking

## Topology

All three VMs use **dual adapters**:

- **Adapter 1 — NAT**: gives each VM internet access (package installs, Windows Update) without exposing them to the physical LAN.
- **Adapter 2 — Host-Only (`vboxnet0`)**: the private subnet the VMs use to reach *each other*. Default gateway/host address is `192.168.56.1`.

```
┌─────────────────────────────────────────────────────────┐
│                  Host-Only Network (vboxnet0)             │
│                     192.168.56.0/24                       │
│                                                             │
│   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐ │
│   │ Kali Linux    │   │ Ubuntu        │   │ Windows      │ │
│   │ .2x  (attacker)│   │ .1x (target)  │   │ .3x (target) │ │
│   └──────┬───────┘   └──────┬───────┘   └──────┬───────┘ │
│          │                   │                   │         │
└──────────┼───────────────────┼───────────────────┼─────────┘
           │                   │                   │
      NAT (internet)      NAT (internet)      NAT (internet)
```

## Setting up the Host-Only network in VirtualBox

1. **File → Tools → Network Manager → Host-only Networks → Create**
2. Confirm it's bound to `192.168.56.0/24` (VirtualBox's default) — if you need a different range, set it here, not per-VM.
3. For each VM: **Settings → Network → Adapter 2 → Enable → Attached to: Host-only Adapter → vboxnet0**

## Verifying connectivity

Once all three VMs are up, confirm every VM can reach every other VM over the host-only adapter:

```bash
# From Kali
ping -c 3 <ubuntu-host-only-ip>
ping -c 3 <windows-host-only-ip>
```

```powershell
# From Windows
ping <kali-host-only-ip>
```

If a ping fails, check in this order: (1) the adapter is attached to the same host-only network name on both VMs, (2) the VM's firewall isn't blocking ICMP, (3) `ip a` / `ipconfig` actually shows an address on `192.168.56.0/24` — a missing address usually means the adapter setting didn't save, not a routing problem.

> **Why not bridged networking?** Bridged mode would put these intentionally-vulnerable VMs on the same network as the host's real LAN. Host-only keeps all attack/target traffic contained to the hypervisor, which is the whole point of an isolated lab.
