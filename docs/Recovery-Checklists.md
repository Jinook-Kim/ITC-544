# Recovery Procedures and Checklists

This section provides **simple, step-by-step recovery procedures** for major systems and **quick checklists** to follow during a disaster. The goal is to ensure anyone on the IT team can restore critical services quickly and consistently.

---

# 1. General Recovery Workflow

### **Step-by-Step Process**

1. **Identify the failed system** (server, service, application, network device).
2. **Check the most recent backup** (snapshot, config file, database dump).
3. **Restore the system** using the appropriate method (VM restore, file restore, rebuild from template).
4. **Verify functionality** (login works, services run, network connectivity).
5. **Document the restore** in the Testing/Verification Logs.
6. **Notify stakeholders** once the system is operational.

---

# 2. System-Specific Recovery Procedures

## 2.1 Active Directory (AD) Recovery

Used when AD-PRIMARY fails or authentication stops working.

### **Steps**

1. Power on **AD-BACKUP** (secondary DC) if offline.
2. Confirm replication health on the backup DC:
   - `repadmin /replsummary`
3. If AD-PRIMARY is corrupted:
   - Restore from VM snapshot **or**
   - Rebuild using Server 2022 template.
4. Rejoin the restored server to the domain.
5. Confirm AD authentication and DNS resolution.
6. Verify Group Policies apply correctly.

---

## 2.2 DNS Server Recovery (BIND9)

### **Steps**

1. Restore VM from most recent backup **or** rebuild from template.
2. Restore zone files from backup (daily/hourly backups).
3. Restart DNS service:
   - `systemctl restart bind9`
4. Test name resolution:
   - `nslookup ad1.sysadmin.local`
   - `nslookup google.com`
5. Confirm VLANs can resolve internal addresses.

---

## 2.3 MySQL Database Recovery

### **Steps**

1. Restore the database VM snapshot **or** rebuild from template.
2. Apply most recent full database backup.
3. Apply transaction logs (for point-in-time recovery).
4. Validate database integrity:
   - Check tables and schema.
5. Test database connectivity from the web server.

---

## 2.4 Web Server Recovery (Apache)

### **Steps**

1. Restore Apache VM from snapshot **or** rebuild from template.
2. Restore `/var/www` and config files from backup.
3. Restart Apache:
   - `systemctl restart apache2`
4. Test internal and external access.
5. Verify firewall rules allow traffic (ports 80/443).

---

## 2.5 File Server Recovery (TrueNAS)

### **Steps**

1. Restore storage pool snapshot.
2. Verify share permissions.
3. Confirm SMB/NFS services start correctly.
4. Test access from multiple VLANs.
5. Restore individual files from snapshots if needed.

---

## 2.6 Firewall/Router (OPNsense) Recovery

### **Steps**

1. Restore VM snapshot **or** import last known good configuration.
2. Confirm:
   - VLAN interfaces
   - DHCP scopes
   - Firewall rules
   - NAT rules
3. Test connectivity:
   - Gateway ping
   - Internal VLAN routing
   - External Internet
4. Re-enable VPN and DNS services.

---

## 2.7 VPN Server Recovery

### **Steps**

1. Restore VM from template or backup.
2. Reapply WireGuard/OpenVPN configuration.
3. Confirm UDP port forwarding on firewall.
4. Test client connection using a known-good config.
5. Ensure internal DNS resolution works through VPN.

---

## 2.8 SIEM Server (Wazuh) Recovery

### **Steps**

1. Restore VM snapshot **or** rebuild from template.
2. Restore configuration and past logs if needed.
3. Confirm agent check-in.
4. Test alerts and dashboards.

---

## 2.9 Workstation/Client Recovery

### **Steps**

1. Reimage workstation using standard image.
2. Join domain.
3. Apply user profile data if backed up.
4. Test login, mapped drives, and policies.

---

# 3. Quick Disaster Recovery Checklists

## 3.1 Immediate Response Checklist

- [ ] Confirm incident type (hardware failure, cyberattack, etc.)
- [ ] Notify IT manager and DR team
- [ ] Secure affected area or isolate compromised systems
- [ ] Review Monitoring/Alerts for root cause indicators
- [ ] Begin recovery procedures based on system priority

---

## 3.2 System Recovery Checklist

- [ ] Identify failed system
- [ ] Check documented RTO/RPO requirements
- [ ] Locate most recent valid backup
- [ ] Restore VM/data/configuration
- [ ] Verify service functionality
- [ ] Perform health checks (logs, connectivity, authentication)
- [ ] Document recovery steps in log
- [ ] Notify affected departments

---

## 3.3 Post-Recovery Checklist

- [ ] Confirm system stability for 24 hours
- [ ] Review logs for anomalies
- [ ] Update documentation if steps changed
- [ ] Conduct brief team review (What worked? What didn’t?)
- [ ] Close incident report

---

# Summary

These procedures and checklists ensure the IT team can recover critical systems quickly, consistently, and according to defined RTO/RPO goals. They provide simple, repeatable steps that work for both routine failures and full disaster scenarios.
