# Milestone 2: VM and Network Setup

This milestone focused on deploying and verifying the team’s virtualized infrastructure. All physical and virtual machines were installed from ISO images within the assigned Proxmox instance. A functional network was established with VLAN segmentation, firewall enforcement, and verified connectivity between key systems. Configuration followed the principle of least privilege and documented VLAN assignments, IP addressing, and Proxmox organization. All systems were tested for connectivity, security policy compliance, and operational stability, with snapshots and backups taken for recovery.

## 1. Technologies & Software

## 2. Network Design

## 3. Hosts Inventory

## 4. Router

## 5. Firewall Rules

## 6. Hardware Planning

### Cable Documentation

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

#### Cable Verification

All cables were tested using a Fluke cable tester. Each passed continuity, pinout, and cross-talk validation.  
Cables **C1** and **C2** were bonded for **LACP** to enable link aggregation between the Proxmox server and the physical switch.
