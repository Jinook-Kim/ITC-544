# Disaster Recovery (DR) Policy

The Disaster Recovery (DR) Policy ensures the organization can maintain or quickly restore critical technology services after disruptions such as cyber incidents, hardware failures, or natural disasters.

## 1. Risk Assessment and Business Impact Analysis (BIA)
The BIA identifies which systems are critical and determines the potential impact of their failure.

**A. Criticality Tiers**

| **Tier** | **System** | **Rationale (Impact of Failure)** |
|----------|------------|------------------------------------|
| **Tier 1 (Mission-Critical)** | Active Directory (Primary & Backup) | Authentication and identity fail. All server services, user logins, and permissions cease. |
| | DNS Server (BIND9) | Name resolution fails. Internal network navigation and external resource access are immediately disrupted. |
| | Database Server (MySQL) | Core business data lost. Application functionality ceases, and all transactional data is unavailable. |
| **Tier 2 (Business-Critical)** | SIEM Server (Wazuh) | Security blind spot: loss of real-time detection, logging, and incident response capabilities, significantly increasing risk. |
| | Web Server (Apache) | Public presence lost. Corporate communication and external services become unavailable. |
| | File Server (TrueNAS Core) | Employee productivity halts. Centralized documents and shared resources become inaccessible. |
| | Router/Firewall (OPNsense) | Network segmentation and external access fail. Complete loss of internet and internal network integrity. |
| **Tier 3 (Supporting/Operational)** | VPN Server | Remote access fails. Halts hybrid/remote work, but onsite operations continue. |
| | Metric Server (Graphite) | Monitoring loss. Inability to track resource usage or detect future performance issues. |
| | Onsite Backup Server | Local recovery options lost, forcing reliance on slower offsite backups. |
| | Vulnerability Scanner | Inability to perform security assessments, though production remains operational. |
| | Workstations (All Clients) | Individual productivity loss. |


## 2. Recovery Objectives and Priorities
Recovery Objectives define the target timeframes for recovery. RTO (Recovery Time Objective) is the maximum acceptable downtime. RPO (Recovery Point Objective) is the maximum tolerable data loss, measured backward in time.

| System                      | Criticality Tier | RTO (Time to Restore Service) | RPO (Max Data Loss)                  | Priority (Order of Restoration) |
|------------------------------|-----------------|-------------------------------|--------------------------------------|--------------------------------|
| Active Directory (AD)        | T1              | ≤1 Hour (Failover)            | 0 Hours (Immediate Replication)      | 1 (Highest)                    |
| DNS Server (BIND9)           | T1              | ≤2 Hours                      | 1 Hour (High caching/zone file changes) | 3                             |
| Database Server (MySQL)      | T1              | ≤4 Hours                      | 15 Minutes (High Transaction Rate)  | 4                              |
| Router/Firewall (OPNsense)   | T2              | ≤2 Hours                      | 4 Hours (Configuration changes)      | 2                              |
| SIEM Server (Wazuh)          | T2              | ≤6 Hours                      | 1 Hour (Log retention requirement)   | 5                              |
| Web Server (Apache)          | T2              | ≤6 Hours                      | 4 Hours (Code/content updates)       | 6                              |
| File Server (TrueNAS)        | T2              | ≤8 Hours                      | 4 Hours (Standard document changes) | 7                              |
| VPN Server                   | T3              | ≤12 Hours                     | 24 Hours (Static config)             | 8                              |
| Onsite Backup Server         | T3              | ≤48 Hours                     | 24 Hours (Backup configuration)      | 9 (Lowest)                     |

## 3. Disaster Recovery (DR) Team Roles and Responsibilities

| DR Role                               | Core Responsibility                                                                                                     | Technical Focus (Your Systems)                                                                                       |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| 1. DR Team Lead (Incident Commander)  | Overall Crisis Management. Declares the disaster, authorizes recovery spending, manages external communication (vendors, media), and ensures RTOs are met. | Confirms status of Tier 1 Systems (AD/DNS/DB) and initiates failover/restoration procedures.                         |
| 2. Communications Lead (Documentation/Audit) | Internal & External Communication. Notifies staff, stakeholders, and tracks all recovery steps and logging for post-incident audit/compliance. | Manages the SIEM Server (T2) log retention/archival if the SIEM is still partially functional.                        |
| 3. Infrastructure/Network Specialist  | Foundational Restoration. Restores network connectivity, segmentation, and perimeter security.                          | Priority 2: Restores Router/Firewall (OPNsense) configuration. Priority 3: Restores DNS Server (BIND9) functionality. |
| 4. Systems/Identity Specialist        | Identity and Core Services. Restores authentication, critical servers, and operating systems.                             | Priority 1: Manages AD Failover (Primary to Backup). Restores/rebuilds virtual machines (VMs) for all Tier 1/T2 systems. |
| 5. Data & Application Specialist      | Data Integrity and Application Functionality. Restores data from the Onsite/Offsite Backup Servers and validates application functionality. | Priority 4: Restores Database Server (MySQL) from high-frequency logs/backup. Restores File Server (TrueNAS) data.    |

## 4. Contact Information Registry

### A. Internal DR Team Contacts (Tier 1 Priority)

| Role                              | Name   | Primary Phone   | Backup Phone        | Email        | On-Call Hours        |
|----------------------------------|--------|----------------|-------------------|-------------|--------------------|
| 1. DR Team Lead                   | [Name] | [Direct Mobile] | [Home/Alternative] | [Work Email]| 24/7               |
| 2. Communications Lead            | [Name] | [Direct Mobile] | [Alternative]      | [Work Email]| 24/7               |
| 3. Systems/Identity Specialist    | [Name] | [Work Mobile]   | [Alternative]      | [Work Email]| 24/7               |
| 4. Infrastructure/Network Specialist | [Name] | [Work Mobile] | [Alternative]     | [Work Email]| 24/7               |
| 5. Data & Application Specialist  | [Name] | [Work Mobile]   | [Alternative]      | [Work Email]| M-F, Business Hours|
| Backup AD Administrator           | [Name] | [Work Mobile]   | [Alternative]      | [Work Email]| 24/7               |

### B. Critical Vendor and Service Provider Contacts

| Vendor/Service       | Contact Name/Dept | Primary Phone       | Service Contract # | Role/Dependency                        |
|---------------------|-----------------|-------------------|------------------|---------------------------------------|
| Internet/ISP         | Technical Support | [ISP 24/7 Number] | [Account Number]  | WAN/External Connectivity             |
| Cloud/Offsite Backup | [Vendor Name] Support | [Support Number] | [Contract ID]     | Offsite Data Recovery                  |
| Hardware Supplier    | Technical Support | [Supplier Contact] | [Warranty ID]     | Replacement of Server/Firewall Hardware|
| OPNsense Consultant  | [Consultant Name] | [Contact Number] | N/A               | Firewall Configuration/Troubleshooting|
| Power/Utility        | Emergency Line   | [Local Utility Number] | N/A           | Power Outage Resolution               |

### C. Emergency Services and Facilities

| Service               | Contact/Department     | Primary Phone         | Address (Site Location)           |
|-----------------------|----------------------|---------------------|---------------------------------|
| Fire Department        | Emergency Dispatch    | 911 (or local equivalent) | [Building Address]             |
| Police/Security        | Non-Emergency Line    | [Local Police Number] | [Building Address]              |
| Local Medical          | Emergency Dispatch    | 911 (or local equivalent) | [Closest Hospital Address]     |
| Facilities Management  | Building Maintenance  | [Contact Number]      | N/A                             |

## 5. Data Backup and Restoration Procedures
### A. Restoration Principles
- Systems are restored based on criticality tier and priority
- Restores must meet defined RTO and RPO objectives
- Data integrity and service functionality must be validated post-restore
- Only IT_Admins are authorized to initiate restoration activities

### B. Restoration Process
- Confirm incident scope and affected systems
- Identify the most recent valid backup meeting RPO
- Restore system infrastructure (VM, OS, networking)
- Restore application data and configuration
- Validate service availability and access
- Document recovery actions and outcomes

### C. Tier Alignment
- Tier 1: Replication, transaction logs, and nightly images
- Tier 2: Snapshots and incremental backups
- Tier 3: Configuration backups and scheduled images

## 6. Disaster Recovery Site

## 7. Hardware and Software Inventory
| System                  | Role                          | OS/Platform            | Hardware Type          | Replacement Strategy (Source/Method)                                                                                     |
|-------------------------|------------------------------|-------------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------|
| AD-P & AD-B             | Primary & Backup AD DS        | Windows Server 2022     | Virtual Machine (VM)    | Failover & Template: Immediate failover to Backup AD. Rebuild using standardized Server 2022 VM template.                |
| Router/Firewall         | Network Core/Security         | Hardened BSD (OPNsense) | Virtual Machine (VM)    | Configuration Backup: Restore configuration to a pre-staged OPNsense VM/Appliance.                                       |
| DNS Server              | BIND9 Name Resolution         | Ubuntu Server           | Virtual Machine (VM)    | VM Template: Rebuild VM. Restore zone files from backup (RPO ≤ 1 hr).                                                     |
| Database Server         | MySQL Core Data               | Ubuntu Server           | Virtual Machine (VM)    | Data Stream: Restore from high-frequency transaction logs (RPO ≤ 15 mins).                                                |
| SIEM Server             | Wazuh Security Monitoring     | Kali Purple             | Virtual Machine (VM)    | VM Template: Rebuild VM from template. Restore configuration and archive historical logs.                                 |
| Web Server              | Apache Website Host           | Ubuntu Server           | Virtual Machine (VM)    | Codebase Backup: Rebuild VM from template. Restore code and configuration (RPO ≤ 4 hrs).                                  |
| File Server             | TrueNAS Central Storage       | TrueNAS Core            | Virtual Machine (VM)    | Data Backup: Rebuild VM from template. Restore data from incremental backup (RPO ≤ 4 hrs).                                |
| VPN Server              | Secure Remote Access          | Ubuntu Server           | Virtual Machine (VM)    | Configuration: Rebuild VM from template. Restore WireGuard configuration.                                                 |
| Onsite Backup Server    | Local Backup Store            | Linux/Backup OS         | Virtual Machine (VM)    | Lowest Priority: Verify access to offsite storage immediately.                                                            |
| Metric Server           | Graphite Monitoring           | Ubuntu Server           | Virtual Machine (VM)    | Rebuild: Restore monitoring configuration and application.                                                                |
| Vulnerability Scanner   | Security Testing              | Kali Purple             | Virtual Machine (VM)    | Rebuild: Reinstall application and configuration.                                                                         |
| Windows Clients (5x)    | User Workstations             | Windows 10              | Physical                | Physical/Image Deployment: Replace hardware. Deploy standardized Windows 10 golden image.                                 |
| Linux Client (Workstation) | User Workstation         | Debian Desktop 13.1.0   | Physical                | Physical/Image Deployment: Deploy standardized Debian image.                                                              |
| Linux Client (Admin)    | Admin Workstation             | Ubuntu Desktop          | Physical    | VM Template: Restore using standard Ubuntu VM template.                                            

## 8. Network Infrastructure
### A. Network Components
- Router/Firewall: OPNsense
- Segmented internal network using VLANs
- VPN access via WireGuard
- DNS services provided by BIND9

### B. Network Restoration Procedure
1. Restore OPNsense configuration from safely stored and encrypted backup
2. Verify WAN connectivity and routing (Static and Dynamic Routes)
3. Re-enable VLAN segmentation and firewall policies
4. Restore VPN access for administrators only before rolling out to other employees
5. Validate DNS resolution across all confiugred VLANS and internal VLAN connectivity

### C. Security Controls
- Firewall rules restored prior to exposing applications
- Least-privilege access enforced during recovery process
- Network activity closely monitored via SIEM

## 9. Application Recovery Procedures
### A. Application Dependencies
- Active Directory → DNS → Database → Applications
- Web and file services depend on identity services
- SIEM depends on restored log pipelines and agent connectivity

### B. Recovery Steps
1. Rebuild application host VM on proxmox
2. Restore application binaries and configuration
3. Restore application data
4. Validate service startup and access
5. Monitor logs for errors or anomalies
6. Verify that everything is in its place as it was before recovery
7. Verify recovery documentation is up to date after successfuly recovery

## 10. Testing and Maintenance
- Quarterly tabletop disaster recovery exercises
- Annual live recovery simulation
- Testing includes VM restoration, network recovery, and backup validation
- Documentation on backup procedures and policy closely studied and updated based on test outcomes

## 11. Training and Awareness
- DR roles documented and communicated to staff
- Annual DR awareness training for IT personnel
- Train employees on safe storage to avoid personal data loss
- Role-specific technical training for DR team members
- New staff onboarded with DR orientation

## 12. Vendor and Supplier Contingency Plans
- Critical vendors must maintain documented recovery capabilities
- Service Level Agreements (SLAs) reviewed annually
- Backup vendors included in DR testing
- Alternate suppliers identified for critical hardware and services

## 13. Financial and Legal Considerations
 Emergency recovery spending pre-authorized
- Insurance coverage reviewed annually
- Compliance requirements include:
  - NIST SP 800-34 and 800-53
  - HIPAA (HR data)
  - GDPR (Sales and BI data)
- Legal counsel engaged for data breach or regulatory incidents

## 14. Emergency Response Procedures
### A. Immediate Actions
- Ensure personnel safety (Main Priority)
- Isolate affected systems if required to avoid further escalation
- Preserve evidence for forensic investigation
- Notify the DR Team Lead

### B. Incident Types Covered
- Cyber incidents (ransomware, intrusion)
- Power failures
- Hardware failures
- Natural disasters

## 15. Incident Reporting and Escalation
- Incidents reported immediately to the DR Team Lead
- Escalation based on scope, severity, and data sensitivity
- SIEM used to support incident timeline reconstruction
- All actions logged for audit and compliance purposes

## 16. Communication Plan
### A. Internal Communication
- Staff notified via approved internal channels
- Regular status updates provided during recovery

### B. External Communication
- Vendors and partners notified as required
- Compliance and legal notifications handled by leadership
- Public communications approved prior to release

## 17. Post-Recovery Evaluation
- Post-incident review conducted within 10 business days
- Review includes:
  - Timeline analysis
  - RTO/RPO compliance
  - Communication effectiveness
  - Technical gaps
- Lessons learned documented
- Policies and procedures updated accordingly





