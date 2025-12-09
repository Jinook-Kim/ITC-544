# 🏛️ Enterprise Network Security & Infrastructure Project

This documentation provides a complete overview of the enterprise network design, implementation, and maintenance framework — including identity management, network security, monitoring, disaster recovery, and verification.

All milestone documentation is included below in one unified structure.

---

## 🧩 Network & Systems Design

Establishes the foundational architecture of the enterprise network, covering physical and logical topology, hardware planning, policies, and host documentation.

| Page | Description |
|------|-------------|
| [Network Design](docs/Network-Design.md) | Logical network topology, VLAN segmentation, routing design, and IP addressing. |
| [Hardware Planning](docs/Hardware-Planning.md) | Server hardware selection, redundancy planning, and performance justification. |
| [Logical Topology Diagram](docs/Logical.png) | Visual representation of logical network segmentation. |
| [Physical Topology Diagram](docs/Physical.png) | Physical layout of network devices and connectivity. |
| [Hosts Inventory](docs/Hosts-Inventory.md) | Inventory of all servers, workstations, and network appliances. |
| [Policies](docs/Policies.md) | Organizational policies and regulatory frameworks to enforce compliance. |
| [Technologies & Software](docs/Technologies-and-Software.md) | Operating systems, applications, and services used across the network. |

---

## 👥 Directory Services & Access Control

Defines the authentication, authorization, and directory service design, ensuring secure and centralized access control for users and devices.

| Page | Description |
|------|-------------|
| [Active Directory & LDAP](docs/Active-Directory-and-LDAP.md) | Domain structure, organizational units, LDAP integration, and authentication methods. |
| [Users & Groups](docs/Users-And-Groups.md) | User role definitions, group hierarchies, and access tier documentation. |
| [Group Policies](docs/Group-Policies.md) | System, security, and hardening GPO enforcement policies. |
| [Assumptions & Justifications](docs/Assumptions-and-Justifications.md) | Design decisions and architectural assumptions. |
| [File Server](docs/File-Server.md) | File sharing configuration, NTFS permissions, and access enforcement. |

---

## 🌐 Network Services & Security Controls

Documents the core network services and access control rules that define the secure communication and segmentation structure of the network.

| Page | Description |
|------|-------------|
| [DHCP](docs/DHCP.md) | DHCP scopes, reservations, and IP lease management. |
| [DNS](docs/DNS.md) | DNS zones, records, and redundancy configuration. |
| [Router](docs/Router.md) | Routing policies, NAT behavior, and gateway configuration. |
| [Firewall Rules](docs/Firewall-Rules.md) | Principle of least privilege firewall rule set for VLANs and external traffic. |
| [File Server](docs/File-Server.md) | Shared directory permissions and access control. |
| [Policies](docs/Policies.md) | Operational and compliance frameworks governing network behavior. |

---

## 📊 Monitoring, SIEM & VPN

Covers system visibility, real-time security monitoring, and remote access. It integrates SIEM tools, endpoint protection, and VPN solutions for secure connectivity.

| Page | Description |
|------|-------------|
| [SIEM & Endpoint Protection](docs/SIEM-Endpoint-Protection.md) | Wazuh deployment, endpoint agent configuration, and alert correlation. |
| [Metric Server](docs/Metric-Server.md) | Resource monitoring, uptime metrics, and dashboard visualization. |
| [VPN Server](docs/VPN-Server.md) | WireGuard VPN configuration, key exchange, and secure access setup. |
| [Vulnerability Scanning](docs/Vulnerability-Scanning.md) | Network vulnerability assessment and remediation tracking. |
| [Technologies & Software](docs/Technologies-and-Software.md) | Tools and utilities supporting monitoring and security operations. |
| [Firewall Rules](docs/Firewall-Rules.md) | Updated rule sets securing SIEM and VPN communication channels. |

---

## 🛡️ Business Continuity & Disaster Recovery

Ensures continuity of operations and organizational resilience through structured backup, restoration, and disaster recovery planning.

| Page | Description |
|------|-------------|
| [Backup Policy](docs/Backup-Policy.md) | Comprehensive backup strategy including retention periods, backup types, encryption, automation, off-site storage, and the 3-2-1-1-0 rule. |
| [Disaster Recovery Policy](docs/Disaster-Recovery-Policy.md) | Framework covering risk assessment, RTO/RPO definitions, system recovery procedures, and communication plans. |
| [Testing & Verification Logs](docs/Testing-Verification.md) | Backup restore testing, verification schedules, audit logs, and validation reports. |
| [Recovery Procedures & Checklists](docs/Recovery-Checklists.md) | Step-by-step restoration procedures, checklists, and escalation paths. |

---


## ✅ Deployment Readiness Status

All milestones and documentation pages have been completed, tested, and verified.  
Each section aligns with grading requirements for design, security, and continuity documentation.
