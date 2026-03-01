# Secure Server Infrastructure
## OPNsense Firewall, Nginx Reverse Proxy and Node.js Backend

---

# 1. Goal

Building a secure infrastructure with three virtual machines for practical implementation of:

- Linux administration
- Network planning
- Firewall configuration
- Reverse proxy
- systemd services

---

# 2. Network Design

![Architecture Diagram](diagram.png)

## IP Plan

| System       | Role               | IP Address |
|-------------|-------------------|------------|
| OPNsense     | Firewall / Router | 10.0.0.254 |
| Web Server   | Reverse Proxy     | 10.0.0.10  |
| App Server   | Backend           | 10.0.0.20  |
| Windows Host | Management        | 10.0.0.1   |

Subnet: 10.0.0.0/24



---

# 3. VM1 – OPNsense

## VM Configuration

- OS: OPNsense (FreeBSD 64-bit)
- CPU: 2 Cores
- RAM: 4 GB
- Disk: 20 GB
- Adapter 1: Bridged (WAN)
- Adapter 2: VMnet1 (LAN)

## Interface

WAN → em0  
LAN → em1  
LAN IP → 10.0.0.254/24  

## Firewall

LAN → any → pass  

## Quick Test

- https://10.0.0.254 reachable
- WAN receives IP
- Ping to 8.8.8.8 not successful

Cause: VMware Bridged Adapter was set to "Automatic" and used the wrong network adapter.

Solution: In VMware → Virtual Network Editor → Select the correct WLAN/Ethernet adapter for VMnet0.

---

# 4. VM2 – Web Server (Ubuntu + Nginx)

## VM Configuration

- OS: Ubuntu Server 22.04
- CPU: 2 Cores
- RAM: 2 GB
- Disk: 20 GB
- Network: VMnet1

---

## 4.1 Static IP (During Installation)

IPv4 Method: Manual

- Subnet: 10.0.0.0/24
- Address: 10.0.0.10
- Gateway: 10.0.0.254
- DNS: 1.1.1.1, 8.8.8.8

After installation check:
