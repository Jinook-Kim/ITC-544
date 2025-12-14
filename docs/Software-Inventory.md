# Software Inventory

This document provides a complete inventory of all software deployed across the enterprise environment. It includes operating systems, server roles, client applications, management tools, and supporting services used to operate the wiki infrastructure and associated network systems.

---

## 1. Operating Systems

| System Type        | Operating System & Version | Purpose / Notes                               |
| ------------------ | -------------------------- | --------------------------------------------- |
| Domain Controllers | Windows Server 2019        | Active Directory, DNS, Group Policy           |
| File Server        | Windows Server 2019        | File sharing, NTFS permissions, backups       |
| Web Server         | Ubuntu Server 20.04 LTS    | Wiki hosting, web services                    |
| Database Server    | Ubuntu Server 20.04 LTS    | SQL database hosting for the wiki             |
| Workstations       | Windows 10 / Windows 11    | User access, testing, administrative tasks    |
| Security Systems   | Ubuntu Server (various)    | SIEM, endpoint monitoring, logging appliances |

---

## 2. Server Roles & Services

| Server Role         | Software / Service                              | Description                                           |
| ------------------- | ----------------------------------------------- | ----------------------------------------------------- |
| Domain Services     | Active Directory Domain Services (AD DS)        | Centralized authentication and authorization          |
| File Services       | Windows File Services                           | SMB shares, permissions, quotas                       |
| Web Hosting         | Apache2 or Nginx _(choose based on deployment)_ | Hosts the wiki application                            |
| Database            | MariaDB / MySQL                                 | Stores wiki backend data                              |
| DNS                 | Windows DNS Server                              | Internal DNS resolution for AD-integrated zones       |
| DHCP                | Windows DHCP Server                             | IP address assignment, reservations, scope management |
| VPN                 | OpenVPN                                         | Secure remote-access VPN for administrators           |
| Endpoint Protection | Windows Defender / SIEM agents                  | Malware protection and centralized monitoring         |
| Logging & Analytics | SIEM Platform                                   | Log collection, analysis, alerts                      |
| Backup Services     | Windows Server Backup / rsync                   | Scheduled backups for DR compliance                   |

---

## 3. Application Software

| Category             | Software                            | Description                                   |
| -------------------- | ----------------------------------- | --------------------------------------------- |
| Wiki Platform        | MediaWiki or similar                | Main documentation and knowledgebase platform |
| Administrative Tools | RSAT Tools (Windows)                | AD, DHCP, DNS, and Group Policy management    |
| Database Management  | phpMyAdmin / MySQL CLI tools        | Database administration                       |
| Network Tools        | Wireshark, Nmap                     | Troubleshooting and assessment tools          |
| Security Tools       | Sysmon, Security Baseline Templates | Logging and hardening tools                   |

---

## 4. User Applications

| User Type         | Software                           | Purpose                                  |
| ----------------- | ---------------------------------- | ---------------------------------------- |
| Standard Users    | Web browser, wiki client           | Access to internal wiki resources        |
| Administrators    | PowerShell, SSH client, RDP        | Server and service management            |
| Security Analysts | SIEM dashboards, endpoint consoles | Monitoring, detection, incident response |

---

## 5. Version Management & Updates

| Component         | Policy                                                         |
| ----------------- | -------------------------------------------------------------- |
| Operating Systems | Updated monthly through Windows Update or apt repositories     |
| Security Tools    | Updated automatically via SIEM or Defender                     |
| Wiki Platform     | Updated quarterly to ensure security and feature compatibility |
| Database System   | Updates scheduled during maintenance windows                   |
| Server Roles      | Updated as recommended by vendor baselines                     |

---

## 6. License & Compliance Tracking

| Software          | License Type                                                     | Notes                                     |
| ----------------- | ---------------------------------------------------------------- | ----------------------------------------- |
| Windows Server    | Volume License                                                   | Required for AD, file services            |
| Windows Client OS | Enterprise License                                               | Workstations                              |
| SIEM Platform     | Open-source or enterprise subscription (depending on deployment) |
| OpenVPN           | Open-source                                                      | No additional licensing required          |
| Web Server Stack  | Open-source                                                      | Includes Apache/Nginx, PHP, MySQL/MariaDB |

---

This software inventory should be updated whenever new applications, services, or system versions are deployed within the environment.
