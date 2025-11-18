# DHCP

## Overview

The DHCP service is responsible for dynamically assigning IP addresses to client workstations within VLAN 214 (Workstations) and ensuring seamless integration with DNS and Active Directory for name resolution and authentication.

## DHCP Server Details

| Server   | IP Address | Scope Name   | Subnet       |
| -------- | ---------- | ------------ | ------------ |
| OPNsense | 10.0.10.11 | Workstations | 10.0.50.0/24 |

## Scope Configuration

| Scope Name   | Start IP   | End IP      | Subnet Mask   | Gateway   | DNS Servers            |
| ------------ | ---------- | ----------- | ------------- | --------- | ---------------------- |
| Workstations | 10.0.50.50 | 10.0.50.200 | 255.255.255.0 | 10.0.50.1 | 10.0.10.11, 10.0.60.10 |

## Reservations

**No DHCP reservations are configured.**  
All devices either receive a dynamic IP from the DHCP pool (workstations) or use a statically assigned IP (servers, infrastructure devices).

## Static Assignments

| Device          | IP Address | Purpose                       |
| --------------- | ---------- | ----------------------------- |
| AD-PRIMARY      | 10.0.10.11 | Primary Domain Controller     |
| AD-BACKUP       | 10.0.10.21 | Backup Domain Controller      |
| GRAPHITE-MATRIX | 10.0.10.15 | Administrative Utility Server |
| WAZUH-SIEM      | 10.0.20.10 | Security Monitoring           |
| MYSQL-DB        | 10.0.30.10 | Database Server               |
| APACHE-WEB      | 10.0.40.10 | Web Server                    |
| DNS-SERVER      | 10.0.60.10 | DNS Resolution                |
| OPENVPN-SERVER  | 10.0.60.11 | VPN Access Gateway            |
