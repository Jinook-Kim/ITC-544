# Testing and Verification Logs

This section explains **how backups and disaster recovery procedures are tested**, and **how results should be logged**. The goal is to keep testing simple, repeatable, and well-documented.

---

## 1. Purpose

- Confirm backups can be successfully restored.
- Ensure recovery steps work as expected.
- Catch issues early (corrupt backups, missing data, broken procedures).
- Provide proof that the organization is meeting RTO/RPO goals.

---

## 2. What We Test

### **2.1 File Restore Test**

- Restore one file from a backup (e.g., HR, Sales, IT).
- Make sure the file opens and the data is correct.

### **2.2 VM/Server Restore Test**

- Restore a VM snapshot or configuration backup into a test environment.
- Confirm the system boots and services run normally.

### **2.3 Database Restore Test**

- Import a database backup into a test instance.
- Verify tables load and queries work.

### **2.4 Full DR Practice Test**

- Simulate the loss of a major system (AD, DNS, DB, Web).
- Walk through documented recovery steps.
- Make sure the team can perform the recovery without issues.

---

## 3. Testing Schedule

| Test Type     | How Often | Purpose                              |
| ------------- | --------- | ------------------------------------ |
| File Restore  | Weekly    | Checks basic backup reliability      |
| VM Restore    | Monthly   | Confirms full-system restores work   |
| DB Restore    | Monthly   | Ensures critical data is recoverable |
| DR Simulation | Quarterly | Tests real-world recovery ability    |

---

## 4. How to Run a Test

### **4.1 Basic Restore Test**

1. Pick a recent backup.
2. Restore it to a test folder or test VM.
3. Verify it works (open file, boot VM, load database).
4. Log the result.

### **4.2 DR Simulation Test**

1. Pick a system to “fail” (AD, DNS, MySQL, etc.).
2. Follow the recovery steps in the DR policy.
3. Time how long recovery takes.
4. Log what worked, what failed, and what needs updating.

---

## 5. Log Template (Copy/Paste)

### Test Entry

Date:
Tester:
Test Type: (File/VM/DB/DR Simulation)
System Tested:
Backup Used:
Actions Taken:

- Step 1
- Step 2
- Step 3

Result: Pass / Fail
Issues Found:
Restore Time:
Comments:

---

## 6. Evidence to Collect

- Screenshot of successful restore
- Backup log output (if available)
- Notes on errors or missing documentation

---

## 7. If Something Fails

1. Mark the test as **FAILED**.
2. Identify the issue (bad backup, incorrect steps, missing data).
3. Fix the problem (update process, repair backup job).
4. Re-test within 1–2 days.
5. Update documentation if needed.

---

## 8. Storage of Logs

All testing logs should be saved in the file server.

Access limited to IT staff.

---

## Summary

This testing process ensures:

- Backups work
- Recovery steps stay accurate
- Staff know how to restore systems
- The organization meets its RTO/RPO goals

Simple, consistent testing prevents surprises during real outages.
