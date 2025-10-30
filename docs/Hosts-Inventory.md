# Hosts Inventory

This document lists all hosts in the environment, including hostnames, IP addresses, VLANs, roles, and statuses.  

---

## VLAN 210 – Admin / Management (10.0.10.0/24)  
**Description:** Housing all core administrative infrastructure.  

| Hostname        | IP Address     | VLAN | Role                     | Status   |
|-----------------|---------------|------|--------------------------|----------|
| AD-PRIMARY      | 10.0.10.10    | 210  | Windows Server 2022 (Primary AD) | Planned  |
| AD-BACKUP       | 10.0.10.11    | 210  | Windows Server 2022 (Backup AD)  | Planned  |
| ADMIN-WS-UBUNTU | 10.0.10.20    | 210  | Ubuntu Desktop (Admin Workstation) | Planned|

---

## VLAN 211 – Security (10.0.20.0/24)  
**Description:** Dedicated zone for monitoring, threat detection, and security analysis.  

| Hostname        | IP Address     | VLAN | Role                          | Status   |
|-----------------|---------------|------|-------------------------------|----------|
| WAZUH-SIEM      | 10.0.20.10    | 211  | IDS/IPS/SIEM                  | Planned  |
| KALI-PURPLE     | 10.0.20.15    | 211  | Security Analysis Workstation | Planned  |

---

## VLAN 212 – Servers (10.0.30.0/24)  
**Description:** Backend services and applications that are not administrative or public-facing.  

| Hostname     | IP Address     | VLAN | Role                | Status   |
|--------------|---------------|------|---------------------|----------|
| MYSQL-DB     | 10.0.30.10    | 212  | MySQL Database      | Planned  |
| TRUENAS-FS   | 10.0.30.20    | 212  | File Server (SMB/NFS) | Planned|

---

## VLAN 213 – Web Server (10.0.40.0/24)  
**Description:** DMZ for public-facing web services.  

| Hostname   | IP Address     | VLAN | Role              | Status   |
|------------|---------------|------|-------------------|----------|
| APACHE-WEB | 10.0.40.10    | 213  | Apache Web Server | Planned  |

---

## VLAN 214 – Workstations (10.0.50.0/24)  
**Description:** Standard user devices.  

| Hostname   | IP Address     | VLAN | Role              | Status   |
|------------|---------------|------|-------------------|----------|
| WIN-WS-01  | 10.0.50.10    | 214  | Windows 10 Client | Planned  |
| WIN-WS-02  | 10.0.50.11    | 214  | Windows 10 Client | Planned  |
| WIN-WS-03  | 10.0.50.12    | 214  | Windows 11 Client | Planned  |
| WIN-WS-04  | 10.0.50.13    | 214  | Windows 11 Client | Planned  |
| WIN-WS-05  | 10.0.50.14    | 214  | Windows 10 Client | Planned  |
| DEBIAN-WS  | 10.0.50.20    | 214  | Debian Desktop    | Planned  |

---

## VLAN 215 – VPN (10.0.60.0/24)  
**Description:** Remote users connecting to the network via OpenVPN.  

| Hostname       | IP Address     | VLAN | Role                    | Status   |
|----------------|---------------|------|-------------------------|----------|
| OPNsense-GW    | 10.0.60.1     | 215  | Router / Firewall / DNS / DHCP | Planned  |
| VPN-CLIENT-01  | 10.0.60.50    | 215  | Remote User (OpenVPN)   | Planned  |
| VPN-CLIENT-02  | 10.0.60.51    | 215  | Remote User (OpenVPN)   | Planned  |

---

## VLAN 216 – Backup Server (10.0.70.0/24)  
**Description:** Dedicated backup zone.  

| Hostname   | IP Address     | VLAN | Role           | Status   |
|------------|---------------|------|----------------|----------|
| DUPLICATI  | 10.0.70.10    | 216  | Backup Server  | Planned  |

---

# Notes  
- IPs shown are representative and may be adjusted during deployment.  
- Status “Active” assumes hosts are in production. Testing or planned hosts should be marked as “Planned” or “Deployed.”  
- This inventory should be updated quarterly or whenever hosts are added/retired.  
