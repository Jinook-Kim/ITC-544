# Initial Design Documentation

## Technologies & Software

| Component                  | Operating System            | Software          |
| -------------------------- | --------------------------- | ----------------- |
| Router/Firewall            | Hardened BSD (via OPNsense) | OPNsense          |
| DNS Server                 | Ubuntu Server               | BIND9 / BIND      |
| SIEM                       | Kali Purple                 | Wazuh             |
| Web Server                 | Ubuntu Server               | Apache            |
| VPN Server                 | Ubuntu Server               | OpenVPN           |
| File Server                | TrueNAS Core                | TrueNAS           |
| Active Directory (Primary) | Windows Server 2022         | AD DS             |
| Active Directory (Backup)  | Windows Server 2022         | AD DS             |
| Database Server            | Ubuntu Server               | MySQL             |
| Metric Server              | Ubuntu Server               | Graphite          |
| Windows Clients (×5)       | Windows 11                  | User Workstations |
| Linux Client (Workstation) | Debian Desktop 13.1.0       | User Workstation  |
| Linux Client (Admin)       | Ubuntu Desktop              | Admin Workstation |

# Disaster Recovery  

Disaster Recovery ensures that the organization can maintain or quickly restore the availability of **critical systems** in the event of hardware failure, cyberattacks, or other outages.  

## Most Critical Systems
- **Active Directory (AD) & DNS** – Provide authentication, permissions, and name resolution.  
- **MySQL Database & Apache Web Server** – Core components of the organization’s web applications.  
- **File Server** – Central storage for shared documents.  
- **VPN Services** – Ensure secure remote access to internal resources.  

## Backup Strategies
- **File Server** – TrueNAS replication for reliable data protection.  
- **Virtual Machines** – Proxmox VM snapshots to allow quick rollbacks after issues or patch failures.  

## Failover Mechanisms
- **Directory & DNS** – Secondary domain controller with DNS services to maintain authentication and resolution.  
- **Databases** – MySQL replication to a standby instance for redundancy.  
- **Core Applications** – Apache and other critical services configured to restart automatically or shift to backup VMs when available.  

## Testing Plan
- **VM Restore** – Test VM restores in Proxmox before attempting in production.  
- **File Recovery** – Periodic recovery scenarios to validate that files can be restored from TrueNAS backups.
- **Schedule** – Run quarterly tabletop exercises and at least one full restore scenario annually.  

---

# Firewall Rules  

Firewall rules follow the **principle of least privilege**, allowing only the traffic required for business operations and blocking everything else by default.  

## Principles
- **Default Deny** – Block all inbound traffic except through the VPN.  
- **Granularity** – Apply rules based on source, destination, protocol, and port.  
- **Documentation** – Each rule includes justification and ownership.  

## Example Rules
1. **Web Access**  
   - Allow TCP 443 (HTTPS) from web server to internet.  

2. **Database Security**  
   - Allow TCP 3306 (MySQL) **only** from the web server to the MySQL server.  

3. **DNS**  
   - Allow UDP/TCP 53 (DNS) requests **only** from internal VLANs to authorized DNS servers.  

4. **VPN Enforcement**  
   - Block all inbound traffic from untrusted networks.  
   - Require VPN access for external connections.  
