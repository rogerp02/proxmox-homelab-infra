# proxmox-homelab-infra

This project demonstrates my hands-on experience with setting up a secure and functional home network lab using Proxmox VE. It includes a virtualized firewall (OPNsense), a DNS filtering server (AdGuard Home), and a Windows Server Active Directory domain controller — all running on a single Proxmox host using a combination of VMs and LXC containers.

---

## Project Overview

- **Host**: Proxmox VE (bare metal)
- **Firewall / Router**: OPNsense VM
- **DNS Filtering**: AdGuard Home in Debian LXC container
- **Active Directory**: Windows Server 2022 VM with AD DS role
- **Network**:
  - `vmbr0`: LAN bridge for internal VMs, containers, and lan net users
  - `vmbr1`: WAN bridge for outbount (ISP DHCP)
- **IP Scheme**:
  - `192.168.5.1` – OPNsense LAN Gateway
  - `192.168.5.2` – Proxmox Host
  - `192.168.5.10` – AdGuard Home
  - `192.168.5.20` – Windows Server (Domain Controller)
  - `192.168.5.30` – WAP #1
  - `192.168.5.31` – WAP #2
  - `192.168.5.32` – WAP #3
  - All clients and infrastructure on the same /24 subnet

---

## Security Features

- **Custom OPNsense firewall rules**:
  - Only trusted admin machines can access the web GUIs of Proxmox, OPNsense, and AdGuard.
  - DNS traffic is restricted: all LAN devices are forced to use the AdGuard DNS server (port 53 only allowed to `192.168.5.10`).
- **Static DHCP Reservations**:
  - All critical systems (e.g., servers, admin workstations) have fixed IPs with aliases for easier rule management.
- **Blocklists & Filtering**:
  - AdGuard Home uses community ad-blocking lists to filter trackers, ads, and known malicious domains at the DNS level.
- **Isolated Web Interfaces**:
  - Web access to infrastructure services is not exposed to the public internet and is locked down to internal admin IPs only.

---

## DNS Design

- **Internal Devices** use AdGuard Home (`192.168.5.10`) for DNS resolution.
- **AdGuard Upstreams** use secure DNS-over-HTTPS (DoH) to Cloudflare or other privacy-focused resolvers.
- **No device certificates required**, keeping LAN DNS simple while upstream queries remain encrypted and private.

---

## Active Directory Lab

A Windows Server 2022 VM acts as a Domain Controller (DC) for a simulated small business environment. The domain is used to model basic organizational structure and user access controls.

- **Domain**: `corp.local`
- **Roles Installed**: Active Directory Domain Services (AD DS), DNS
- **Organizational Units**:
  - HR
  - Accounting
  - IT
  - Executives
- **Users & Groups**:
  - Test users created with realistic names and departments
  - Security groups assigned based on department roles
  - Role-based access using group policy and delegated admin rights
- **Group Policies**:
  - Password complexity/enforcement
  - Login scripts
  - Basic desktop restrictions for non-admin users
- **Purpose**:
  - To simulate a functioning domain environment for training, experimentation, and system administration practice

---

## Platform Details

| Component         | Type        | Description                                       |
|------------------|-------------|---------------------------------------------------|
| Proxmox VE       | Hypervisor  | Base platform managing all VMs and containers     |
| OPNsense         | VM          | Handles WAN connectivity, firewalling, NAT, DHCP |
| AdGuard Home     | LXC         | DNS-level ad and tracker blocking                 |
| Windows Server   | VM          | Active Directory domain controller                |

---

## Skills Demonstrated

- Proxmox VM and LXC provisioning
- OPNsense firewall configuration
- LAN/WAN segmentation and bridge setup
- Secure DNS implementation with DoH upstreams
- DHCP static mappings and alias management
- Windows Server 2022 installation and configuration
- Active Directory domain setup and group policy management
- Organizational IT modeling with realistic structures

---

## Planned Improvements

- Add backups and snapshots for OPNsense and AdGuard
- VLAN support once managed switch is available

---

## Summary

This project reflects my ability to design, deploy, and secure a virtualized IT infrastructure using Proxmox VE. From routing and DNS to identity and access management, this setup simulates core systems commonly found in business networks. It serves both as a learning platform and as evidence of my readiness for entry-level IT roles in system administration, networking, or technical support.
