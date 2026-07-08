# Homelab Overview

A cybersecurity and IT administration homelab running on a repurposed office PC, started July 4, 2026. Built as a hands-on environment for practicing defensive security and sysadmin skills, and as a portfolio piece.

Full write-up with reasoning, hardware specs, and environment details: [Homelab-Proxmox-and-Pi-hole-Environment-Documentation-July-8th.pdf](Homelab-Proxmox-and-Pi-hole-Environment-Documentation-July-8th.pdf)

## Why this project exists

Built to have a real, hands-on environment for cybersecurity and IT administration practice, rather than just studying concepts in isolation. It doubles as a portfolio piece since employers in defensive security and sysadmin roles care about seeing actual infrastructure work, not just certifications or coursework. It's also a genuinely good vehicle for honing skills from University coursework and experimenting with areas I enjoy.

## Why Proxmox

Chose Proxmox VE over alternatives like ESXi or running everything in Docker on bare Linux for two main reasons. It's free and open source, so there's no licensing cost or restriction getting in the way of experimenting. It also gives real flexibility between full VMs and lightweight LXC containers on the same host, mirroring the kind of virtualization environment expected in an actual IT admin or sysadmin role. Prior academic experience with Proxmox also allowed for an accelerated deployment timeline.

## Why an old office PC instead of dedicated hardware

The machine hosting all of this is a repurposed office PC rather than something bought specifically for homelabbing. This kept the cost of the project at basically zero, and forces real thinking about resource constraints, working with 16GB of RAM total rather than an unlimited compute budget. Deciding what runs full time versus on demand is its own kind of practical systems administration skill.

Total hardware cost: $0.

## Physical setup

Sits next to my main desktop. Runs headless, no monitor, mouse, or keyboard plugged in, everything is managed through the Proxmox web UI over the local network.

## Hardware specifications

| Component | Detail |
|---|---|
| CPU | Intel Core i5-6600 @ 3.30GHz, 4 cores, 1 socket |
| RAM | 15.54 GiB total |
| Storage | 1TB external SSD |
| Swap | 8.00 GiB |
| Network interfaces | enp0s31f6 (onboard NIC), enx5065f34d1da3 (added USB NIC) |
| Boot mode | EFI |
| Hypervisor | Proxmox VE, pve-manager 9.2.2 |
| Kernel | Linux 7.0.2-6-pve |

At the time of writing, the host runs comfortably under load: 0.76% CPU usage across 4 cores, 10.29% RAM usage (1.60 GiB of 15.54 GiB), 4.57% disk usage on the root filesystem, and 0% swap usage. Future VMs like Wazuh will run on demand rather than powered on 24/7.

A second NIC is already in place (the USB adapter), which sets up the future pfSense VM, needing its own dedicated interface separate from general LAN traffic.

![Proxmox summary](screenshots/proxmox-summary.png)

## Environment overview

Currently running one LXC container, named pihole, on Debian 12, documented in [-HOMELAB-DNS-Privacy-Stack](https://github.com/homeslashsean/-HOMELAB-DNS-Privacy-Stack). Uses a single storage pool built on the 1TB external SSD for now. As more VMs get added, storage and RAM allocation will need to be tracked more carefully since the 16GB ceiling is a real constraint.

![Container list](screenshots/container-list.png)

## Security posture

The host itself hasn't been hardened yet. This is next on the list, alongside setting up pfSense. Planned steps include restricting web UI access to LAN only and enabling 2FA on the root account.

## Planned next steps

- Harden the Proxmox host itself (restrict web UI access, enable 2FA)
- pfSense VM
- Wazuh SIEM
- Kali Linux VM
- Jellyfin with GTX 960
