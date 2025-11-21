# DNS

## Overview

The DNS server at **10.0.60.10** provides authoritative DNS for `sysadmin.local` and reverse lookup zones for all infrastructure VLANs. It resolves internal services (AD, SIEM, DB, Web, VPN, Backup) using hostname mappings and forwards external DNS queries to **8.8.8.8** and **1.1.1.1**.  
It supports Active Directory by resolving domain controllers (`ad1`, `ad2`) and ensuring domain-joined machines can authenticate.

## DNS Server Configuration

| Server     | IP Address | Role (Primary/Secondary) | Software/Version |
| ---------- | ---------- | ------------------------ | ---------------- |
| dns-server | 10.0.60.10 | Primary DNS Server       | BIND9 (Ubuntu)   |

## Zone Configuration

| Zone Type      | Zone Name             | Records Included        | Notes                        |
| -------------- | --------------------- | ------------------------ | ---------------------------- |
| Forward Lookup | sysadmin.local        | SOA, NS, A              | Authoritative forward zone   |
| Reverse Lookup | 10.0.10.in-addr.arpa  | PTR                      | Admin VLAN                   |
| Reverse Lookup | 10.0.20.in-addr.arpa  | PTR                      | Security VLAN                |
| Reverse Lookup | 10.0.30.in-addr.arpa  | PTR                      | Database VLAN                |
| Reverse Lookup | 10.0.40.in-addr.arpa  | PTR                      | Web Server VLAN              |
| Reverse Lookup | 10.0.60.in-addr.arpa  | PTR                      | VPN / DNS VLAN               |
| Reverse Lookup | 10.0.70.in-addr.arpa  | PTR                      | Backup VLAN                  |

## Record Examples

| Record Type | Name    | Value      | TTL    | Description                    |
| ----------- | -------- | ---------- | ------ | ------------------------------ |
| A           | dns     | 10.0.60.10 | 604800 | Primary DNS server             |
| A           | ad1     | 10.0.10.11 | 604800 | Primary Domain Controller      |
| A           | ad2     | 10.0.10.21 | 604800 | Backup Domain Controller       |
| A           | website | 10.0.40.10 | 604800 | Apache Web Server              |
| A           | siem    | 10.0.20.10 | 604800 | Wazuh SIEM (Kali Purple)      |
| A           | db      | 10.0.30.10 | 604800 | MySQL Database Server          |
| A           | vpn     | 10.0.60.11 | 604800 | OpenVPN Server                 |
| A           | metric  | 10.0.70.10 | 604800 | Graphite Metrics/Backup Server |
| CNAME       | www     | website    | 604800 | Web server alias               |
| MX          | @       | ad1        | 604800 | (If mail service is configured)|

## Forwarding & Resolution

### Forwarders used

```
8.8.8.8
1.1.1.1
```

### Internal resolution tests

```
nslookup ad1.sysadmin.local
nslookup website.sysadmin.local
nslookup siem.sysadmin.local
nslookup db.sysadmin.local
nslookup vpn.sysadmin.local
```

### Reverse resolution tests

```
nslookup 10.0.10.11
nslookup 10.0.40.10
nslookup 10.0.20.10
```

### External resolution test

```
nslookup google.com
```

---

# **BIND9 CONFIGURATION (FULL CONFIGS USED)**

## **named.conf.options**

```bash
options {
    directory "/var/cache/bind";

    recursion yes;
    allow-recursion { 10.0.0.0/16; };

    allow-query {
        10.0.10.0/24;
        10.0.20.0/24;
        10.0.30.0/24;
        10.0.40.0/24;
        10.0.50.0/24;
        10.0.60.0/24;
        10.0.70.0/24;
    };

    listen-on { any; };
    listen-on-v6 { any; };

    forwarders {
        8.8.8.8;
        1.1.1.1;
    };

    dnssec-validation no;
    auth-nxdomain no;
};
```

---

## **named.conf.local**

```bash
zone "sysadmin.local" {
    type master;
    file "/etc/bind/db.sysadmin.local";
};

zone "10.0.10.in-addr.arpa" {
    type master;
    file "/etc/bind/db.10.0.10.rev";
    allow-transfer { none; };
};

zone "20.0.10.in-addr.arpa" {
    type master;
    file "/etc/bind/db.10.0.20.rev";
    allow-transfer { none; };
};

zone "30.0.10.in-addr.arpa" {
    type master;
    file "/etc/bind/db.10.0.30.rev";
    allow-transfer { none; };
};

zone "40.0.10.in-addr.arpa" {
    type master;
    file "/etc/bind/db.10.0.40.rev";
    allow-transfer { none; };
};

zone "60.0.10.in-addr.arpa" {
    type master;
    file "/etc/bind/db.10.0.60.rev";
    allow-transfer { none; };
};

zone "70.0.10.in-addr.arpa" {
    type master;
    file "/etc/bind/db.10.0.70.rev";
    allow-transfer { none; };
};
```

---

## **db.sysadmin.local**

```bash
$TTL    604800
@       IN      SOA     dns.sysadmin.local. admin.sysadmin.local. (
                        5
                        604800
                        86400
                        2419200
                        604800 )

@       IN      NS      dns.sysadmin.local.

dns     IN      A       10.0.60.10
ad1     IN      A       10.0.10.11
ad2     IN      A       10.0.10.21
website IN      A       10.0.40.10
siem    IN      A       10.0.20.10
db      IN      A       10.0.30.10
vpn     IN      A       10.0.60.11
metric  IN      A       10.0.70.10
```

---

## **Reverse Zone Files (ALL USED)**

### **db.10.0.10.rev**

```bash
$TTL 604800
@   IN  SOA dns.sysadmin.local. admin.sysadmin.local. (
        1 604800 86400 2419200 604800 )
    IN  NS  dns.sysadmin.local.
11  IN  PTR ad1.sysadmin.local.
21  IN  PTR ad2.sysadmin.local.
```

### **db.10.0.20.rev**

```bash
$TTL 604800
@   IN  SOA dns.sysadmin.local. admin.sysadmin.local. (
        1 604800 86400 2419200 604800 )
    IN  NS  dns.sysadmin.local.
10  IN  PTR siem.sysadmin.local.
```

### **db.10.0.30.rev**

```bash
$TTL 604800
@   IN  SOA dns.sysadmin.local. admin.sysadmin.local. (
        1 604800 86400 2419200 604800 )
    IN  NS  dns.sysadmin.local.
10  IN  PTR db.sysadmin.local.
```

### **db.10.0.40.rev**

```bash
$TTL 604800
@   IN  SOA dns.sysadmin.local. admin.sysadmin.local. (
        1 604800 86400 2419200 604800 )
    IN  NS  dns.sysadmin.local.
10  IN  PTR website.sysadmin.local.
```

### **db.10.0.60.rev**

```bash
$TTL 604800
@   IN  SOA dns.sysadmin.local. admin.sysadmin.local. (
        1 604800 86400 2419200 604800 )
    IN  NS  dns.sysadmin.local.
10  IN  PTR dns.sysadmin.local.
11  IN  PTR vpn.sysadmin.local.
```

### **db.10.0.70.rev**

```bash
$TTL 604800
@   IN  SOA dns.sysadmin.local. admin.sysadmin.local. (
        1 604800 86400 2419200 604800 )
    IN  NS  dns.sysadmin.local.
10  IN  PTR metric.sysadmin.local.
```

