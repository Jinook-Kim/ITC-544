Assumptions & Justifications

The assumptions made during the network and VM setup, along with justifications for the design choices related to VLANs, IP addressing, firewall rules, routing, and administrative accounts are as follows:

⸻

VLAN Segmentation
	•	The network is segmented into seven functional VLANs (210–216) to reduce broadcast domains, improve performance, and increase security.
	•	Each VLAN supports a specific purpose:
	•	210 – Admin/Management: Core servers (Active Directory, Metrics). Restricted to IT staff.
	•	211 – Security: Wazuh SIEM for centralized monitoring and intrusion detection. Other VLANs can send logs here, but inbound access is blocked.
	•	212 – Database: MySQL and TrueNAS. Only the web VLAN has access on port 3306.
	•	213 – Web Server: DMZ hosting Apache for public access. Only HTTP/HTTPS traffic is allowed from WAN.
	•	214 – Workstations: Windows and Linux clients use DHCP; limited to internal services and internet.
	•	215 – Networking: OpenVPN and DNS services for remote access.
	•	216 – Backup: Duplicati VM isolated for backup traffic to prevent cross-contamination in case of compromise.
	•	This segmentation follows the principle of least privilege and defense-in-depth, ensuring a compromise in one VLAN does not affect others.

⸻

IP Addressing
	•	All VLANs use /24 subnets for simplicity, scalability, and easy troubleshooting.
	•	Gateways are always assigned to the first address in each subnet (e.g., 10.0.10.1, 10.0.20.1, etc.).
	•	Servers use static IPs for consistent connectivity, while workstations and VPN clients use DHCP scopes managed by OPNsense.
	•	Using the 10.0.x.0 private range ensures internal addressing remains isolated from the WAN network (172.16.47.0/21).

⸻

Firewall Rules
	•	The OPNsense firewall enforces a “deny-all by default” policy with explicit allow rules for required services.
	•	Key traffic allowed includes:
	•	DNS (UDP/TCP 53) to the internal DNS server.
	•	HTTP/HTTPS (TCP 80/443) from WAN → Web Server VLAN.
	•	MySQL (TCP 3306) from Web → Database VLAN.
	•	AD/DNS/RDP traffic from Workstations → Admin VLAN.
	•	Log forwarding from all VLANs → Security VLAN.
	•	All other inter-VLAN traffic is blocked, and logging is enabled for deny rules to detect unauthorized attempts.
	•	This ensures proper isolation while maintaining essential business and administrative communication.

⸻

Routing & Services
	•	OPNsense acts as the edge router, handling DHCP, NAT, and inter-VLAN routing.
	•	DNS resolution is managed by the Unbound Resolver, forwarding to external servers (1.1.1.1 and 8.8.8.8).
	•	A single WAN interface (172.16.47.2/21) provides outbound connectivity for all internal VLANs.
	•	The routing table confirms all subnets are reachable through VLAN interfaces (vtnet1_vlan210–216).
	•	This setup provides a simple, stable, and maintainable routing structure without introducing unnecessary complexity.

⸻

Local Backup Admin Accounts
	•	Each system includes a localadmin account for emergency access.
	•	This account is not domain-linked, allowing login even if Active Directory or network connectivity fails.
	•	Credentials are stored securely in a shared team password vault.
	•	The approach follows standard sysadmin best practices and prevents lockout during domain or network issues.

⸻

Hardware, Proxmox, and Organization
	•	The Proxmox host uses LACP bonding (Cables C1 & C2) for redundancy and bandwidth aggregation.
	•	Each VM’s resources are allocated based on its function, with snapshots created after installation for rollback capability.
	•	VMs are grouped using Proxmox pools for organization:
	•	network – Router, DNS, OpenVPN
	•	security – AD, SIEM, Metrics
	•	application – Web Server, MySQL
	•	Tags (e.g., router, web, ad, vpn) help with filtering, monitoring, and resource tracking.
	•	This structure keeps the environment organized, scalable, and easy to maintain.

⸻