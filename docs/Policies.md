# Organizational IT Security Policies

## 1. Purpose  
The purpose of these policies is to establish clear guidelines for the secure and responsible use of organizational IT systems. These policies support confidentiality, integrity, and availability across servers, workstations, networks, and cloud resources.  

## 2. Scope  
These policies apply to all employees, contractors, and third parties who access organizational IT resources, including servers, workstations, VPN connections, and data storage platforms.  


## 3. Policies  

### 3.1 Acceptable Use Policy (AUP)  
- Users must only access systems and resources necessary for their role.  
- Personal or unauthorized use of company servers, databases, and file shares is prohibited.  
- Installation of unauthorized software or hardware is not allowed.  
- Users must not attempt to bypass security controls, firewalls, or monitoring systems.  

### 3.2 Password & Authentication Policy  
- Passwords must meet minimum complexity standards (12+ characters, mix of upper/lowercase, numbers, and symbols).  
- Multi-factor authentication (MFA) is required for VPN and administrative access.  
- Credentials must not be stored in plain text or shared between users.  

### 3.3 Access Control Policy  
- Role-Based Access Control (RBAC) will be enforced through Active Directory.  
- The principle of “least privilege” applies: users only receive access required for their job duties.  
- Access reviews will be conducted quarterly to remove stale or unused accounts.  
- Administrative accounts must not be used for day-to-day tasks.  

### 3.4 Data Retention & Disposal Policy  
- Logs, backups, and system data must follow defined retention schedules.  
- SIEM data will be retained for 6–12 months, unless required for investigations.  
- Old data must be securely wiped or destroyed using industry-standard practices.  
- Portable media and retired drives must be physically destroyed or sanitized.  

### 3.5 Remote Access Policy  
- All remote connections must use the approved VPN (OpenVPN) with MFA.  
- Split tunneling is prohibited; all traffic must route through the VPN.  
- Only company-approved devices may connect to organizational systems.  
- Remote sessions must be logged and monitored.  

### 3.6 Firewall & Network Segmentation Policy  
- All inbound traffic is denied by default, with only necessary services explicitly allowed.  
- Firewall rules must include:  
  - Allow HTTPS (443) from web server to internet.  
  - Allow MySQL (3306) only from web server to database server.  
  - Allow DNS (53) only from internal VLANs to DNS servers.  
- VLAN segmentation will separate servers, workstations, and administrative systems.  
- Firewall rules will be reviewed at least semi-annually
