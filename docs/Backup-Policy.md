# Part 1: Backup Policy (DR-Aligned Final Version)

## 1. Purpose

This Backup Policy defines how organizational data is regularly copied,
stored, protected, and maintained to ensure rapid recovery following
accidental deletion, corruption, ransomware, hardware failure, or
natural disaster. This policy is directly aligned with the
organization's Disaster Recovery (DR) Policy and supports
mission-critical systems hosted in the Proxmox environment, including
identity services, databases, file storage, security monitoring, and
remote access infrastructure.

------------------------------------------------------------------------

## 2. Key Elements

## 2.1 Data Retention

Retention is aligned to regulatory exposure, operational value, and
departmental risk.

  Data Class                    System / Share       Retention
  ----------------------------- -------------------- --------------
  Identity & Authentication     Active Directory     3 Years
  DNS Zone & Config Files       BIND9                1 Year
  Database Transactions         MySQL                3 Years
  HR Records & PII              HR Share             3 Years
  Sales & Financial Data        Sales Share          3 Years
  Executive Data                Directors Share      3 Years
  IT Scripts & Configurations   IT Share             2 Years
  BI & Reporting Data           BI Share             2 Years
  Marketing & Creative Assets   Graphics / Content   1--2 Years
  Public Organizational Data    Public Share         6--12 Months
  Security Logs                 Wazuh SIEM           90 Days

------------------------------------------------------------------------

## 2.2 Frequency of Backups (DR Criticality Aligned)

### Tier 1 -- Mission-Critical Systems

-   Active Directory (Primary & Backup)
-   DNS (BIND9)
-   Database Server (MySQL)

  System             DR RPO       Backup Method
  ------------------ ------------ ------------------------------------------
  Active Directory   0 Hours      Continuous replication + daily image
  DNS                1 Hour       Hourly config + daily full
  MySQL              15 Minutes   Transaction log streaming + nightly full

------------------------------------------------------------------------

### Tier 2 -- Business-Critical Systems

-   Web Server (Apache)
-   File Server (TrueNAS)
-   Router / Firewall (OPNsense)
-   Wazuh SIEM

  System     DR RPO    Backup Method
  ---------- --------- ----------------------------------------
  Apache     4 Hours   Daily incremental + weekly full
  TrueNAS    4 Hours   Hourly snapshots + nightly replication
  OPNsense   4 Hours   Daily encrypted config backup
  Wazuh      1 Hour    Hourly log export + nightly image

------------------------------------------------------------------------

### Tier 3 -- Operational / Supporting Systems

-   VPN Server (WireGuard)
-   Metric Server (Graphite)
-   Onsite Backup Server
-   Vulnerability Scanner
-   Workstations

  System          DR RPO     Backup Method
  --------------- ---------- -----------------------------
  VPN             24 Hours   Daily config backup
  Metrics         24 Hours   Daily config + weekly full
  Backup Server   24 Hours   Weekly system image
  Clients         7 Days     Weekly profile/image backup

------------------------------------------------------------------------

## 2.3 Backup Types

-   Full Backups
-   Incremental Backups
-   Continuous Replication (AD)
-   Transaction Log Backups (MySQL)
-   Snapshot Backups (Proxmox / TrueNAS)

------------------------------------------------------------------------

## 2.4 Storage Media

-   Proxmox Local Backup Storage
-   TrueNAS Encrypted ZFS Pools
-   Onsite Backup Server
-   Encrypted Cloud Object Storage (off-site)

------------------------------------------------------------------------

## 2.5 Off-Site Backups

-   Tier 1 systems replicated weekly
-   Tier 2 systems archived monthly
-   Geographically separated cloud storage

------------------------------------------------------------------------

## 2.6 Encryption and Security

-   AES-256 encryption at rest
-   TLS 1.2+ encryption in transit
-   Role-Based Access Control (RBAC)
-   IT_Admins only restore authority
-   Least-privilege service accounts

------------------------------------------------------------------------

## 2.7 Testing and Verification

-   Monthly TrueNAS file restores
-   Quarterly Proxmox VM restores
-   Annual full Tier 1 recovery simulation

------------------------------------------------------------------------

## 2.8 Backup Automation

-   Proxmox automated VM jobs
-   TrueNAS scheduled snapshots
-   Cron-based MySQL dumps and firewall config exports

------------------------------------------------------------------------

## 2.9 Monitoring and Alerts

-   Backup success/failure alerts
-   Storage threshold warnings
-   Unauthorized access detection via SIEM

------------------------------------------------------------------------

## 2.10 Documentation and Review

-   Backup schedules
-   Storage locations
-   Encryption standards
-   Restore procedures
-   Retention policies

Reviewed annually or after infrastructure changes.

------------------------------------------------------------------------

## 2.11 Scalability and Growth

-   Monthly storage utilization reviews
-   Cloud bucket expansion on demand
-   TrueNAS ZFS pool expansion

------------------------------------------------------------------------

## 2.12 Compliance and Regulatory Requirements

-   NIST SP 800-34 & 800-53
-   HIPAA (HR Data)
-   GDPR (Sales & BI Data)
-   Internal company security policies

------------------------------------------------------------------------

## 2.13 Disaster Recovery Planning Integration

  System             DR RTO       Backup Support
  ------------------ ------------ --------------------------
  Active Directory   ≤ 1 Hour     Continuous replication
  DNS                ≤ 2 Hours    Hourly backups
  MySQL              ≤ 4 Hours    15-min log backups
  Firewall           ≤ 2 Hours    Daily config export
  SIEM               ≤ 6 Hours    Hourly log backups
  Web Server         ≤ 6 Hours    Daily incremental
  File Server        ≤ 8 Hours    Hourly TrueNAS snapshots
  VPN                ≤ 12 Hours   Daily config backup

------------------------------------------------------------------------

## 2.14 Regular Review and Testing

-   Bi-annual policy review
-   Quarterly tabletop DR exercises
-   Annual full live recovery test
