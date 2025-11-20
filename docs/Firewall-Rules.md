# Firewall Rules

Firewall rules follow the principle of **least privilege**, allowing only the traffic required for business operations and blocking everything else by default.

## Principles
- **Default Deny** – Block all inbound traffic except through the VPN or explicitly permitted rules.  
- **Granularity** – Apply rules based on source, destination, protocol, and port.  
- **Documentation** – Each rule includes justification and ownership.  

## VLAN-Specific Rules

### VLAN 210 – Admin/Management (10.0.10.0/24)
**Hosts/Services**: Windows Server 2022 (Primary AD, Backup AD), Ubuntu Admin Workstation  
- The Admin/Management VLAN provides privileged access for system administration. Outbound traffic is restricted to essential management services across other VLANs, including SSH, RDP, SMB, MySQL, and web management ports. The VLAN is permitted to access Servers (212), Workstations (214), Security (211), and the Web Server (213) only on approved management ports. All other lateral movement is blocked, and an explicit deny rule prevents any inbound traffic from other internal VLANs. 

**Changed Rules**:

Allow DNS to Networking net (53 TCP/UDP)
- Purpose: Allows Admin VLAN to query DNS servers in the Networking VLAN.

Allow LDAP (389 TCP) to Workstations & Networking nets
- This is a new rule giving AdminManagement VLAN LDAP access to domain-joined machines and/or your domain controllers.
- Purpose: Domain authentication, AD queries, management.
<img width="1008" height="519" alt="image" src="https://github.com/user-attachments/assets/75acd32d-e705-466d-b770-94586b14e03d" />


---

### VLAN 211 – Security (10.0.20.0/24)
**Hosts/Services**: Wazuh IDS/IPS/SIEM, Kali Purple  
- Allow **all other VLANs → Security (211)** for log forwarding.  
- Allow **Admin/Management (210) → Security (211)** for administration (e.g., SSH, web console).  
- Deny **all other traffic**.
<img width="518" height="216" alt="image" src="https://github.com/user-attachments/assets/2ac7544d-bf2d-4c7d-b6d4-5b61aa8f6908" />


---

### VLAN 212 – Databse (10.0.30.0/24)
**Hosts/Services**: MySQL Database, TrueNAS File Server   
- Allow **Web Server (213) → Servers (212)** for database access (MySQL only).  
- Deny **all other traffic**.
<img width="515" height="212" alt="image" src="https://github.com/user-attachments/assets/2e985843-347d-4012-8e68-89f41400fd9e" />


---

### VLAN 213 – Web Server (10.0.40.0/24)
**Hosts/Services**: Apache Web Server (DMZ)  
- Allow **WAN → Web Server (213)** on TCP 80/443 only.  
- Allow **Admin/Management (210) → Web Server (213)** on management ports (SSH, RDP if applicable).  
- Deny all inbound traffic from other VLANs.
<img width="514" height="219" alt="image" src="https://github.com/user-attachments/assets/5048aa93-7cc7-403c-b0b1-651edc105a86" />


---

### VLAN 214 – Workstations (10.0.50.0/24)
**Hosts/Services**: Windows Clients (5x), Debian Desktop  
- Allow **Workstations (214) → Web_Ports (80, 443)** for internet access
- Allow **Workstations (214) → Admin/Management (210)** for DNS/AD (TCP/UDP 53, TCP/UDP 389, TCP 445, TCP 3389).  
- Allow **Workstations (214) → Servers (212)** for file sharing (SMB) and databases.  
- Allow **Workstations (214) → Web Server (213)** for internal web pages.  
- Deny **all other inter-VLAN traffic** by default.
<img width="518" height="267" alt="image" src="https://github.com/user-attachments/assets/4018ad60-56c5-4aee-9a04-3eeef7f2edfc" />


---

### VLAN 215 – Networking (10.0.60.0/24)
**Hosts/Services**: OpenVPN clients, OPNsense Router, DNS, DHCP  
- Allow **VPN (215) → Admin/Management (210), Servers (212), and Workstations (214)** on necessary ports (RDP, SMB, SSH, etc.).  
- Deny **all other traffic**.  

---

## Floating Rules 
- **Logging** – All firewall decisions are logged and forwarded to **Wazuh (211)** for auditing.  
- **Monitoring** – Security VLAN (211) receives logs and alerts from all VLANs.
- **DNS** – All internal VLANS --> DNS server (In this case ANY because we don't have our internal DNS server configured)
<img width="521" height="218" alt="image" src="https://github.com/user-attachments/assets/2e9ee374-7ca5-43d2-b39e-cd884b685bec" />
