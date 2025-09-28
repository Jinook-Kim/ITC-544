# Firewall Rules

Firewall rules follow the principle of **least privilege**, allowing only the traffic required for business operations and blocking everything else by default.

## Principles
- **Default Deny** – Block all inbound traffic except through the VPN or explicitly permitted rules.  
- **Granularity** – Apply rules based on source, destination, protocol, and port.  
- **Documentation** – Each rule includes justification and ownership.  

## VLAN-Specific Rules

### VLAN 210 – Admin/Management (10.0.10.0/24)
**Hosts/Services**: Windows Server 2022 (Primary AD, Backup AD), Ubuntu Admin Workstation  
- Allow **Admin/Management → Servers, Workstations, Security, and Web Server VLANs** on management ports (e.g., SSH, RDP, WinRM, AD/DNS).  
- Deny **all inbound traffic** from other VLANs.  

---

### VLAN 211 – Security (10.0.20.0/24)
**Hosts/Services**: Wazuh IDS/IPS/SIEM, Kali Purple  
- Allow **all other VLANs → Security** for log forwarding.  
- Allow **Admin/Management → Security** for administration (e.g., SSH, web console).  
- Deny **all other traffic**.  

---

### VLAN 212 – Servers (10.0.30.0/24)
**Hosts/Services**: MySQL Database, TrueNAS File Server  
- Allow **Workstations (50) → Servers (30)** for services such as SMB (TCP 445) and MySQL (TCP 3306).  
- Allow **Web Server (40) → Servers (30)** for database access (MySQL only).  
- Deny **all other traffic**.  

---

### VLAN 213 – Web Server (10.0.40.0/24)
**Hosts/Services**: Apache Web Server (DMZ)  
- Allow **WAN → Web Server (40)** on TCP 80/443 only.  
- Allow **Admin/Management (10) → Web Server** on management ports (SSH, RDP if applicable).  
- Deny **any outbound traffic** from Web Server VLAN to internal VLANs.  

---

### VLAN 214 – Workstations (10.0.50.0/24)
**Hosts/Services**: Windows Clients (5x), Debian Desktop  
- Allow **Workstations → Admin/Management (10)** for DNS/AD (TCP/UDP 53, TCP/UDP 389, TCP 445, TCP 3389).  
- Allow **Workstations → Servers (30)** for file sharing (SMB) and databases.  
- Allow **Workstations → Web Server (40)** for internal web pages.  
- Deny **all other inter-VLAN traffic** by default.  

---

### VLAN 215 – VPN (10.0.60.0/24)
**Hosts/Services**: OpenVPN clients, OPNsense Router, DNS, DHCP  
- Allow **VPN → Admin/Management, Servers, and Workstations** on necessary ports (RDP, SMB, SSH, etc.).  
- Deny **all other traffic**.  

---

## Global Rules
- **Default Deny** – Any traffic not explicitly allowed is blocked.  
- **Logging** – All firewall decisions are logged and forwarded to **Wazuh** for auditing.  
- **Monitoring** – Security VLAN (211) receives logs and alerts from all VLANs.  

