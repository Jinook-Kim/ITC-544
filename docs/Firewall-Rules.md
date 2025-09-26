# Firewall Rules

Firewall rules follow the **principle of least privilege**, allowing only the traffic required for business operations and blocking everything else by default.

## Principles

- **Default Deny** – Block all inbound traffic except through the VPN.
- **Granularity** – Apply rules based on source, destination, protocol, and port.
- **Documentation** – Each rule includes justification and ownership.

## Example Rules

1. **Web Access (DMZ)**

   - Allow TCP 443 (HTTPS) from WAN → Web Server (VLAN 40).

2. **Database Security**

   - Allow TCP 3306 (MySQL) **only** from Web Server (VLAN 40) → Database Server (VLAN 30).

3. **DNS**

   - Allow UDP/TCP 53 (DNS) requests **only** from internal VLANs → DNS servers (VLAN 10).

4. **VPN Enforcement**

   - Block all inbound traffic from untrusted networks.
   - Require VPN access for all external connections.
   - Explicitly define which VLANs VPN users can access (e.g., AD/DNS, file server) and restrict access to sensitive systems like the database.

5. **Monitoring & Logging**
   - Permit all VLANs → Security VLAN (20) for log forwarding.
   - Ensure firewall events are logged and alerts sent to Wazuh for auditing.
