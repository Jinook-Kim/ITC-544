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
