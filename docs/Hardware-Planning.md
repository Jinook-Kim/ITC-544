# Hardware & System Planning

---

## 1. Physical Workstations & Peripherals

### Workstations (User PCs)
- 7 × Dell OptiPlex Macro PCs  
  - 16 GB RAM  
  - 256–512 GB SSD  

### Admin Workstation
- 1 × Dell OptiPlex 7050  
  - 16 GB RAM  
  - 256 GB SSD  

### Peripherals & Accessories
- 3 × Keyboards  
- 3 × Monitors  
- 3 × Mice  
- 3 × DisplayPort (DP) cables  
- 1 × Switch console cable  
- 8 × 130W power adapters  
- 3 × 14 AWG power cables  
- 1 × Power strip  
- 2 × USB-C to serial cable connectors  

---

## 2. VM Resource Requirements

- **Router / Firewall (OPNsense)** – 2 vCPUs | 4 GB RAM | 20 GB storage  
- **Active Directory (Primary)** – 4 vCPUs | 8 GB RAM | 80 GB storage  
- **Active Directory (Backup)** – 4 vCPUs | 8 GB RAM | 80 GB storage  
- **DNS Server (Debian BIND9)** – 1 vCPU | 1 GB RAM | 20 GB storage  
- **Database Server (Ubuntu MySQL)** – 2 vCPUs | 4 GB RAM | 60 GB storage  
- **Web Server (Ubuntu Apache)** – 2 vCPU | 4 GB RAM | 40 GB storage  
- **File Server (TrueNAS)** – 2 vCPUs | 8 GB RAM | 250 GB storage  
- **VPN Server (Ubuntu OpenVPN)** – 2 vCPU | 4 GB RAM | 20 GB storage  
- **SIEM / IDS-IPS (Kali Purple Wazuh)** – 4 vCPUs | 8 GB RAM | 120 GB storage  
- **Metrics Server (Ubuntu Graphite)** – 1 vCPU | 2 GB RAM | 30 GB storage  
- **Backup & Restore (Debian Duplicati)** – 1 vCPU | 1 GB RAM | 50 GB storage  

---

## 3. RAM
- **Total Required (VMs):** ~47 GB  
- **Recommended Host RAM:** ≥ 64 GB  

---

## 4. CPU
- **Total Required (VMs):** ~23 vCPUs  
- **Recommended Host CPU:** ≥ 12 physical cores (24 threads)  

---

## 5. Storage
- **Total Required (VMs):** ~770 GB  
- **Recommended Host Storage:** ≥ 2 TB usable (RAID/ZFS preferred)  

---

## 6. Network Devices
- **Switches:** 1 × Managed switch  
- **Routers:** OPNsense (Proxmox)  

---

## 7. Backup Storage

### On-Site Backup Capacity & Redundancy
- Proxmox VM for backup  
- Dedicated VLAN for backup traffic, isolated from production  
- **No offsite/cloud backup** — all data retained on a proxmox VM

---

## 8. Cable Documentation

A total of **9 Ethernet cables** were built and tested to ensure proper connectivity and adherence to T568B wiring standards.

| Cable ID | Purpose                    | Connection Points             | Status | Test Result |
| -------- | -------------------------- | ----------------------------- | ------ | ----------- |
| C1       | Switch to Proxmox (Port 1) | Switch Port 1 ↔ Proxmox NIC 1 | Active | Pass        |
| C2       | Switch to Proxmox (Port 2) | Switch Port 2 ↔ Proxmox NIC 2 | Active | Pass        |
| C3       | Workstation 1              | Switch Port 3 ↔ Workstation 1 | Active | Pass        |
| C4       | Workstation 2              | Switch Port 4 ↔ Workstation 2 | Active | Pass        |
| C5       | Workstation 3              | Switch Port 5 ↔ Workstation 3 | Active | Pass        |
| C6       | Workstation 4              | Switch Port 6 ↔ Workstation 4 | Active | Pass        |
| C7       | Workstation 5              | Switch Port 7 ↔ Workstation 5 | Active | Pass        |
| C8       | Workstation 6              | Switch Port 8 ↔ Workstation 6 | Active | Pass        |
| C9       | Workstation 7              | Switch Port 9 ↔ Workstation 7 | Active | Pass        |

### Cable Verification

All cables were tested using a Fluke cable tester. Each passed continuity, pinout, and cross-talk validation.  
Cables **C1** and **C2** were bonded for **LACP** to enable link aggregation between the Proxmox server and the physical switch.

---

## 9. Proxmox Allocation

| VM Name  | Storage                    | RAM                           | CPU    |
| -------- | -------------------------- | ----------------------------- | ------ | 
| OpenVNP  | 20 GB | 4 GB | 2 CPU |
| Router       | 20 GB | 4 GB | 2 CPU | 
| DNS       | 20 GB              | 2 GB | 2 CPU | 
| AD       | 80 GB              | 8 GB | 4 CPU | 
| AD Backup       | 80 GB             | 8 GB | 4 CPU | 
| WebServer       | 20 GB              | 2 GB | 2 CPU | 
| MySqlServer       | 60 GB             | 4 GB | 2 CPU | 
| MetricsServer       | 32 GB              | 4 GB | 2 CPU |
| SIEM       | 120 GB             | 8 GB | 4 CPU |
