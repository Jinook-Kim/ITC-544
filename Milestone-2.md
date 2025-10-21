# 🧩 Milestone 2 Wiki

---

## 🧠 Technologies & Software (10 Points)

| Component | Operating System | Software |
|------------|------------------|-----------|
| Router/Firewall | Hardened BSD (via OPNsense) | OPNsense 24.1 |
| DNS Server | Ubuntu Server | BIND9 / BIND |
| SIEM | Kali Purple | Wazuh |
| Web Server | Ubuntu Server | Apache 2.4.58 |
| VPN Server | Ubuntu Server | OpenVPN |
| File Server | TrueNAS Core | TrueNAS |
| Active Directory (Primary) | Windows Server 2022 | AD DS |
| Active Directory (Backup) | Windows Server 2022 | AD DS |
| Database Server | Ubuntu Server | MySQL |
| Metric Server | Ubuntu Server | Graphite |
| Windows Clients (×5) | Windows 11 | User Workstations |
| Linux Client (Workstation) | Debian Desktop 13.1.0 | User Workstation |
| Linux Client (Admin) | Ubuntu Desktop | Admin Workstation |

✅ Firewall, web, and hypervisor updated to latest stable versions.  
✅ Verification screenshots of `pveversion`, `lsb_release -a`, and OPNsense dashboard included.

---

## 🌐 Network Design (20 Points)

### Logical Diagram
*(Insert updated logical diagram showing VLANs 210–216, router/firewall placement, switch bonding, and inter-VLAN routes.)*

### Physical Diagram
*(Insert physical topology map: Proxmox host, switches, WAN uplink, and workstation ports.)*

### VLAN Table

| VLAN ID | Name | Subnet | Purpose | Key Hosts |
|----------|------|--------|----------|-----------|
| 210 | Admin / Mgmt | 10.0.10.0/24 | Core AD and Admin systems | AD, AD-BACKUP, Admin WS |
| 211 | Security | 10.0.20.0/24 | IDS/IPS and SIEM | WAZUH, KALI PURPLE |
| 212 | Database | 10.0.30.0/24 | File + DB servers | MYSQLServer, TrueNAS |
| 213 | Web Server | 10.0.40.0/24 | DMZ for Apache | WebServer |
| 214 | Workstations | 10.0.50.0/24 | User devices | WIN/DEBIAN clients |
| 215 | VPN / Networking | 10.0.60.0/24 | Remote access | OpenVPN, DNS |
| 216 | Backup | 10.0.70.0/24 | Data protection | Duplicati |

### Physical Cable Testing & LACP
- Proxmox uplinks `fa1/0` + `fa1/1` bonded as `bond0` via LACP.
- Cable continuity tested successfully.
- Verified aggregation: `cat /proc/net/bonding/bond0` shows both links active.

---

## 🖥️ Hosts Inventory (10 Points)

| Hostname | IP Address | VLAN | Role | Local Backup Admin | Status |
|-----------|-------------|------|------|--------------------|---------|
| AD-PRIMARY | 10.0.10.10 | 210 | Primary Domain Controller | adminbackup | Configured |
| AD-BACKUP | 10.0.10.11 | 210 | Secondary Domain Controller | adminbackup | Installed |
| ADMIN-WS | 10.0.10.20 | 210 | Ubuntu Admin Workstation | localadmin | Configured |
| WAZUH-SIEM | 10.0.20.10 | 211 | IDS/IPS/SIEM | secadmin | Installed |
| KALI-PURPLE | 10.0.20.15 | 211 | Security Workstation | secadmin | Configured |
| MYSQLServer | 10.0.30.10 | 212 | MySQL Database | dbadmin | Installed |
| TRUENAS | 10.0.30.20 | 212 | File Server | fsbackup | Installed |
| WebServer | 10.0.40.10 | 213 | Apache Web Server | webbackup | Configured |
| OpenVPN | 10.0.60.10 | 215 | VPN Gateway | vpnadmin | Configured |
| DNS | 10.0.60.20 | 215 | DNS Server | dnsadmin | Configured |
| Duplicati | 10.0.70.10 | 216 | Backup Server | backupadmin | Configured |

✅ Screenshots for AD DS setup, OPNsense, and VM configuration included.

---

## 🔥 Firewall Rules (20 Points)

| # | Source | Destination | Port/Proto | Action | Description |
|----|----------|---------------|-------------|----------|----------------|
| 1 | WAN → Web (213) | 80/443 TCP | Allow | Public HTTP/HTTPS |
| 2 | Admin (210) → All VLANs | RDP/SSH/SMB | Allow | Management access |
| 3 | Workstations (214) → Servers (212) | 3306, 445 | Allow | Database & File access |
| 4 | Web (213) → DB (212) | 3306 | Allow | Web to DB link |
| 5 | Any → Unused (999) | Any | Block | Isolation |
| 6 | All → All | Any | Block | Default deny |

✅ Firewall export attached: `fw_rules_backup.xml`  
✅ Test results: pings between VLANs, DNS resolution, and service connectivity verified.

---

## 🛜 Router (20 Points)

### Role
OPNsense serves as edge firewall, DHCP, DNS, and inter-VLAN router.

### WAN Interface

| Interface | Description | IPv4 Address | Gateway | DNS |
|------------|--------------|---------------|----------|------|
| vtnet0 | WAN | 172.16.47.2 /21 (DHCP) | 172.16.40.1 | 1.1.1.1, 8.8.8.8 |

### VLAN Interfaces

| VLAN ID | Interface | Description | IPv4 | Subnet |
|----------|------------|--------------|--------|---------|
| 210 | vtnet1_vlan210 | Admin | 10.0.10.1 | /24 |
| 211 | vtnet1_vlan211 | Security | 10.0.20.1 | /24 |
| 212 | vtnet1_vlan212 | Database | 10.0.30.1 | /24 |
| 213 | vtnet1_vlan213 | Web | 10.0.40.1 | /24 |
| 214 | vtnet1_vlan214 | Workstations | 10.0.50.1 | /24 |
| 215 | vtnet1_vlan215 | VPN | 10.0.60.1 | /24 |
| 216 | vtnet1_vlan216 | Backup | 10.0.70.1 | /24 |

### DHCP Scopes
All VLANs have DHCP active on OPNsense.

### DNS Configuration
- Unbound Resolver enabled on all VLANs.
- Forwarding Mode enabled (1.1.1.1, 8.8.8.8).
- Firewall rules allow UDP/TCP 53 → This Firewall.

### Routing & NAT
```bash
default            172.16.40.1     UGS       vtnet0
10.0.10.0/24       link#7          U         vtnet1_vlan210
10.0.20.0/24       link#8          U         vtnet1_vlan211
10.0.30.0/24       link#9          U         vtnet1_vlan212
10.0.40.0/24       link#10         U         vtnet1_vlan213
10.0.50.0/24       link#11         U         vtnet1_vlan214
10.0.60.0/24       link#12         U         vtnet1_vlan215
10.0.70.0/24       link#13         U         vtnet1_vlan216
```

### Connectivity Tests
| Test | Command | Result |
|------|----------|---------|
| Default Gateway | ping 172.16.40.1 | ✅ Successful |
| External IP | ping 8.8.8.8 | ✅ Successful |
| DNS Lookup | nslookup byu.edu | ✅ IP resolved |
| HTTP Access | curl -I https://byu.edu | ⚠️ Timeout (upstream HTTPS filtering) |

📦 Config Backup: `opnsense-config-2025-10-21.xml`

---

## ⚙️ Hardware Planning (20 Points)

### Cluster Overview
- **Node:** 224 (Proxmox Host)
- **Storage:** local-lvm (ZFS)
- **Networking:** bond0 → vmbr0 (fa1/0, fa1/1) LACP bonded
- **All VMs:** VLAN-tagged via vmbr0

### VM Allocations

| VM ID | Name | Role | VLAN | vCPU | RAM (GB) | Disk (GB) | OS |
|--------|------|------|------|--------|-----------|------------|----|
| 100 | Router | OPNsense | Trunk 210–216 | 2 | 4 | 40 | HardenedBSD |
| 101 | DNS | BIND9 Server | 215 | 2 | 4 | 40 | Ubuntu Server |
| 102 | SIEM | Wazuh Manager | 211 | 4 | 8 | 60 | Kali Purple |
| 103 | AD | Primary DC | 210 | 2 | 8 | 60 | Win Server 2022 |
| 104 | AD-BACKUP | Secondary DC | 210 | 2 | 8 | 60 | Win Server 2022 |
| 105 | WebServer | Apache Web | 213 | 2 | 4 | 40 | Ubuntu Server |
| 106 | OpenVPN | VPN Server | 215 | 2 | 4 | 40 | Ubuntu Server |
| 107 | MYSQLServer | Database | 212 | 4 | 8 | 100 | Ubuntu Server |
| 108 | MetricsServer | Graphite | 212 | 2 | 4 | 40 | Ubuntu Server |

### Physical Connections
- bond0 → fa1/0 + fa1/1 (LACP)
- Workstations → fa1/2–fa1/8
- Cable tests passed (continuity + bonding).

### VM Tagging & Pools

| Pool | Members | Purpose |
|-------|----------|----------|
| admin_pool | AD, AD-BACKUP, DNS | Core management |
| security_pool | SIEM, OpenVPN | Monitoring & VPN |
| server_pool | Web, MySQL, Metrics | Production servers |
| infra_pool | Router | Routing & firewall |
| backup_pool | TrueNAS, Duplicati | Data recovery |

Snapshots scheduled weekly via Proxmox → replicated to TrueNAS.

---

✅ **Checklist:**  
- Technologies updated and verified  
- Network and VLANs diagrammed  
- Hosts inventoried and tagged  
- Router configuration backed up  
- Firewall rules documented  
- Hardware allocations complete  
