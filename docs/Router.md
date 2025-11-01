## Router

### Role

OPNsense serves as edge firewall, DHCP, DNS, and inter-VLAN router.

### WAN Interface

| Interface | Description | IPv4 Address | Gateway | DNS |
|------------|--------------|---------------|----------|------|
| vtnet0 | WAN | 172.16.47.2 /21 (DHCP) | 172.16.40.1 | 1.1.1.1, 8.8.8.8 |

### VLAN Interfaces

| VLAN ID | Interface | Description | IPv4 | Subnet |
|----------|------------|--------------|--------|---------|
| 210 | vtnet1\_vlan210 | Admin | 10.0.10.1 | /24 |
| 211 | vtnet1\_vlan211 | Security | 10.0.20.1 | /24 |
| 212 | vtnet1\_vlan212 | Database | 10.0.30.1 | /24 |
| 213 | vtnet1\_vlan213 | Web | 10.0.40.1 | /24 |
| 214 | vtnet1\_vlan214 | Workstations | 10.0.50.1 | /24 |
| 215 | vtnet1\_vlan215 | VPN | 10.0.60.1 | /24 |
| 216 | vtnet1\_vlan216 | Backup | 10.0.70.1 | /24 |

### DHCP Scopes

All VLANs have DHCP active on OPNsense.

### DNS Configuration

* Unbound Resolver enabled on all VLANs.
* Forwarding Mode enabled (1.1.1.1, 8.8.8.8).
* Firewall rules allow UDP/TCP 53 → This Firewall.

### Routing \& NAT

```bash
default            172.16.40.1     UGS       vtnet0
10.0.10.0/24       link#7          U         vtnet1\_vlan210
10.0.20.0/24       link#8          U         vtnet1\_vlan211
10.0.30.0/24       link#9          U         vtnet1\_vlan212
10.0.40.0/24       link#10         U         vtnet1\_vlan213
10.0.50.0/24       link#11         U         vtnet1\_vlan214
10.0.60.0/24       link#12         U         vtnet1\_vlan215
```


### ADD CONNECTIVITY TEST EVIDENCE

Screenshot of Workstation pinging to Gateway and Google.
![Web Server Ping Test](./assets/WS%20ping.png)


Admin Rules
![Admin Rules](./assets/Admin%20Rules.png)
Database
![Database Rules](./assets/Database%20Rules.png) 
Network

![Network Rules](./assets/Network%20Rules.png) 

Security

![Security Rules](./assets/Security%20Rules.png)
