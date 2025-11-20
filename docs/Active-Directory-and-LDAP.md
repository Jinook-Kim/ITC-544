# Active Directory & LDAP Setup

## Domain Creation and Configuration

**Primary Domain Controller (DC):**

- Hostname: `AD-PRIMARY`
- IP: `10.0.10.11`

**Backup Domain Controller (DC):**

- Hostname: `AD-BACKUP`
- IP: `10.0.10.21`

**Steps:**

1. Installed the AD DS role on both servers.
2. Promoted `AD-PRIMARY` as the first domain controller for the `sysadmin.local` domain.
3. Joined `AD-BACKUP` to the domain and promoted it as a secondary domain controller.
4. Verified replication and global catalog roles.
5. Configured DNS and forwarders on both controllers.

---

## LDAP Schema and Integration Details

- **Schema version:** 88
- **Base DN:** `DC=sysadmin,DC=local`
- **Key objects:** `user`, `group`, `organizationalUnit`, `computer`
- **Organizational structure:**
  - `FriedChickenCo` (custom OU)
    - `Groups`
    - `Users`
    - `Workstations`
- This structure follows the default Active Directory LDAP schema and organizes directory objects by function for easier management.
- **Integration:** Standard LDAP connection (port **389**) used for user and group authentication and directory queries between clients and the domain controllers (`AD-PRIMARY` and `AD-BACKUP`).

---

## Replication Settings and Verification

- Verified replication between `AD-PRIMARY` and `AD-BACKUP` using:
  ```cmd
  repadmin /showrepl AD-BACKUP
  ```
  ![Replication Settings](assets/Replication%20Settings.png)
