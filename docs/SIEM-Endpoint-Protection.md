Step 4: SIEM & Endpoint Protection
======================================

Using Wazuh (Installed via Official Wazuh Installation Assistant)

Purpose
-------

Deploy a centralized SIEM and endpoint protection framework to collect logs, detect malicious activity, and continuously monitor every device in the network. Wazuh functions as both a SIEM and EDR.

## SIEM Deployment (Wazuh Installation Assistant)


**Documentation Reference:** [https://documentation.wazuh.com/current/installation-guide/wazuh-dashboard/installation-assistant.html](https://documentation.wazuh.com/current/installation-guide/wazuh-dashboard/installation-assistant.html)

The Wazuh Installation Assistant automatically installs:

*   Wazuh Manager
    
*   Wazuh Indexer (OpenSearch)
    
*   Wazuh Dashboard
    
*   Enrollment service
    
*   TLS certificates
    
*   Systemd services
    

### Installation Commands
```bash
curl -sO [https://packages.wazuh.com/4.x/wazuh-install.sh]
sudo bash wazuh-install.sh --wazuh-server wazuh-1
```

### Installed Components & Ports

| Component         | Port            | Purpose               |
|------------------|-----------------|------------------------|
| Wazuh Manager     | 1514/TCP & UDP  | Agent event ingestion |
| Enrollment Service| 1515/TCP        | Agent enrollment      |
| Syslog Listener   | 514/UDP         | Router/Firewall logs  |
| Wazuh Dashboard   | 443/TCP         | Web access            |


### Dashboard Access

https://10.0.20.10

![alt text](image-1.png)

## Endpoint Protection Deployment (Wazuh Agents)


Wazuh Agents were deployed across all required devices to provide real-time monitoring, configuration auditing, malware detection, and file integrity monitoring.

## Agents Installed on Servers

The following servers in the environment have the Wazuh agent installed:

| ID   | Server Name       | IP Address  | Operating System                                      | Agent Version |
|------|-------------------|-------------|--------------------------------------------------------|----------------|
| 001  | AD-primary        | 10.0.10.11  | Windows Server 2022 Standard Evaluation               | v4.14.1        |
| 002  | File-Server       | 10.0.30.15  | Windows Server 2022 Standard Evaluation               | v4.14.1        |
| 003  | AD-Backup         | 10.0.10.21  | Windows Server 2022 Standard Evaluation               | v4.14.1        |
| 005  | OpenVPN-Server    | 10.0.60.11  | Ubuntu 24.04.3 LTS                                     | v4.14.1        |
| 006  | DNS-Server        | 10.0.60.10  | Ubuntu 24.04.3 LTS                                     | v4.14.1        |
| 008  | OPNsense Firewall | 10.0.20.1   | BSD 14.3 (os-wazuh-agent plugin)                      | v4.12.0        |

---

## Agents Installed on Workstations

| ID   | Workstation Name | IP Address  | Operating System                | Agent Version |
|------|------------------|-------------|---------------------------------|----------------|
| 004  | WIN-WS-07        | 10.0.50.12  | Windows 10 Pro                  | v4.14.1        |
| 007  | WIN-WS-04        | 10.0.50.9   | Windows 10 Enterprise           | v4.7.5         |

---
![alt text](<Screenshot 2025-12-08 155326.png>)
---



Windows Agent Installation
--------------------------

```bash
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.1-1.msi -OutFile $env:tmp\wazuh-agent;
msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER='10.0.20.10'
```


Linux Agent Installation
------------------------
```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
sudo bash wazuh-install.sh -a
 ```
---
Enabled EDR Features
===========================

Real-Time Monitoring
--------------------

*   Process creation events
    
*   Privilege escalation attempts
    
*   PowerShell monitoring
    
*   Registry changes
    
*   Authentication events
    
*   Suspicious command execution patterns
    

Scheduled Security Audits (SCA)
-------------------------------

*   CIS Windows Server Benchmark
    
*   CIS Windows 10 Benchmark
    
*   CIS Linux Benchmark
    

File Integrity Monitoring (FIM)
-------------------------------

### Windows

C:\\Windows\\System32\\C:\\Shares\\

### Linux

/etc//var/log//usr/bin/

---
Log Collection Sources
============================



Wazuh is now collecting and centralizing logs from all core infrastructure systems in the lab.  
Below is the full list of monitored devices and the types of logs gathered from each.

---

### **Servers**

| Server | OS | Log Types Collected | Notes |
|-------|-----|----------------------|-------|
| **AD-Primary** | Windows Server 2022 | Security logs, Sysmon events, authentication logs, service events | Detects failed logins, privilege escalation, policy changes |
| **AD-Backup** | Windows Server 2022 | Same as AD-Primary | Provides redundancy and visibility for domain-related events |
| **File-Server** | Windows Server 2022 | File integrity monitoring (FIM), Sysmon, security logs | Detects unauthorized file access or modification |
| **OpenVPN-Server** | Ubuntu 24.04 | Auth logs, VPN connection logs, sudo usage, system logs | Tracks VPN activity and administrative actions |
| **DNS-Server** | Ubuntu 24.04 | BIND logs (if enabled), system logs, auth logs | Monitors DNS queries, errors, and suspicious behavior |
| **OPNsense Firewall** | BSD 14.3 | Firewall events, NAT logs, DHCP logs, system logs | Sent via Syslog → Wazuh SIEM (port **514/UDP**) |

---

### **Workstations**

| Workstation | OS | Logs Collected |
|-------------|----|----------------|
| **WIN-WS-07** | Windows 10 Pro | Sysmon, Security Event Log, FIM |
| **WIN-WS-04** | Windows 10 Enterprise | Sysmon, process creation, PowerShell events |

Both workstations include Wazuh's EDR module (real-time monitoring of processes, threats, and system behavior).

---

### **Network & Router Logs**

| Device | Method | Log Type |
|--------|--------|----------|
| **OPNsense Router** | Syslog → Wazuh | Firewall alerts, packets blocked/allowed, VPN events, intrusion patterns |

---

Alert Configuration
=========================

Alerts were configured for security-critical activity, including failed logins, admin access, file changes, and network attacks.

### Alerts Configured

The following alert categories were configured to detect suspicious or high-risk activity across servers, workstations, and network devices.

| **Category**            | **Condition**                               |
|-------------------------|----------------------------------------------|
| Brute-force attempts    | Multiple failed login attempts               |
| Privileged login        | Domain Admin or root login                   |
| PowerShell abuse        | Encoded or malicious PowerShell commands     |
| FIM alerts              | Protected file modified                      |
| Firewall events         | Network scans, blocked packets               |
| VPN events              | Login failures or successful VPN connections |


Testing & Validation
==========================

Bruteforce Alert : ![alt text](image-2.png)

Required Dashboard Screenshots
====================================

The following screenshots were added to demonstrate SIEM functionality:

*   Wazuh Overview Dashboard
![alt text](image-5.png)
*   Endpoint Summary
![alt text](image-4.png)
*   MITRE ATT&CK Mapping
![alt text](image-6.png)
*   Indexes
![alt text](image-7.png)
*   Alert Timeline
![alt text](image-3.png)

Conclusion
=============

A fully functional SIEM + EDR environment was deployed using the Wazuh Installation Assistant. The system provides real-time monitoring, alerting, file integrity protection, configuration auditing, and centralized event visibility across all servers, workstations, and network devices.
