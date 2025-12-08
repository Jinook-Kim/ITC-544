# 🌐VPN Server: WireGuard Implementation

## 1. Installation Steps and Configuration

### 1.1 Server Preparation and Core Installation
 1. Access and Update: SSH into the server ($\text{10.0.60.11}$) as a sudo user and update the system
    ```
    $ sudo apt update && sudo apt upgrade -y
    ```
2. Install WireGuard: Install the primary package.
```
   $ sudo apt install wireguard -y
```
1.2 Key Generation and File Security (Critical)

WireGuard authenticates using keys, making key security paramount.

1. Generate Private Key: Generate and save the Server Private Key.
   ```
   $ sudo wg genkey | sudo tee /etc/wireguard/server_private.key
   ```
2. Set Secure Permissions (Essential Fix): Restrict access to the private key to root only (600) to prevent file permission issues and ensure security.
   ```
   $ sudo chmod 600 /etc/wireguard/server_private.key
   ```
3. Generate Public Key: Generate the Server Public Key from the private key.
   ```
   $ sudo cat /etc/wireguard/server_private.key | wg pubkey | sudo tee /etc/wireguard/server_public.key
   ```
### 1.3 Networking and Interface Configuration

1. Enable IP Forwarding: This allows the server to route traffic from the VPN tunnel to internal VLANs and the external internet.

```
$ echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.conf
$ sudo sysctl -p
```
2. Create Server Configuration (/etc/wireguard/wg0.conf): Create and populate the configuration file with NAT, routing, and the VPN tunnel subnet ($\text{10.8.0.0/24}$).

```
[Interface]
Address = 10.8.0.1/24       # Server's tunnel IP
SaveConfig = true
PrivateKey = [SERVER_PRIVATE_KEY] 
ListenPort = 51820

# Enable NAT Routing for Internet/VLAN access (using external interface enp1s0)
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o enp1s0 -j MASQUERADE
PreDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o enp1s0 -j MASQUERADE

# [Peer] configurations will be added here for each remote user.
```

3. Start and Enable Service:
```
$ sudo systemctl start wg-quick@wg0.service
$ sudo systemctl enable wg-quick@wg0.service
```
## 2. 🔐 Authentication Methods and Security Policies

WireGuard uses strong, modern Cryptokey Routing for authentication, satisfying the requirement for strong encryption (ChaCha20).

### A. Multi-Factor Authentication (MFA) Enforcement
- Method: MFA Provisioning Gate (Zero Trust Model).

- Process: Since native WireGuard does not support RADIUS/TOTP for connection, users must first authenticate using AD Username/Password + TOTP on an internal system.

- Access Control: Only upon successful MFA verification is the user allowed to download their unique `client.conf` file, which contains their personal, authenticated Private Key. This key then serves as the second factor of authentication for the connection itself.

### B. Auditing and Restriction
- Strong Encryption: WireGuard uses the ChaCha20 cipher, which is a state-of-the-art cipher and satisfies the strong encryption (AES-256 or higher) requirement.

- Logging: All VPN events are logged via the system journal.

```

$ sudo journalctl -u wg-quick@wg0.service

```
- Restriction: Access is granted only to users whose Public Keys are explicitly configured as a `[Peer]` in the server's `wg0.conf` file, preventing unauthorized in-person users from connecting.

## 3. 🧱 Firewall and Port Forwarding Configuration

### 3.1 OPNsense Router Configuration (128.187.49.253)

1. Port Forwarding (NAT): Configure the router to translate traffic from the public IP to the internal server.
   Action: NAT Rule
   Protocol: UDP
   Destination: 128.187.49.253:51820
   Target: 10.0.60.11:51820

2. WAN Firewall Rule: Explicitly allow the inbound traffic.
   Action: Pass (Allow)
   Protocol: UDP
   Destination Port: 51820

3. Internal Routing: Create rules on the VLAN 215 interface to allow the VPN tunnel `10.8.0.0/24` to reach internal resources on required ports `53, 3389, 445, 3306`.

### 3.2 Server Local Firewall (UFW)

Allow WireGuard Port: Allow inbound UDP traffic on the server itself.

```
$ sudo ufw allow 51820/udp
$ sudo ufw reload
```
## 4. 🔗 Connection Instructions for Remote Users

### 4.1 Client Configuration

Users must receive a unique `client.conf` file (obtained after MFA verification) containing the following critical routing and DNS information:
- DNS: Set the internal DNS server: `DNS = 10.0.10.10` (VLAN 210 AD/DNS server).
- AllowedIPs: Configure split tunneling to ensure all internal subnets are accessible via the VPN.
  ```
  AllowedIPs = 10.8.0.0/24, 10.0.10.0/24, 10.0.30.0/24, 10.0.40.0/24, 10.0.50.0/24
  ```
### Example Client Configuration File (client1.conf)

```
[Interface]
# Unique Private Key for the client device (MUST be kept secret)
PrivateKey = [CLIENT_PRIVATE_KEY_GENERATED_BY_SERVER]

# The IP address assigned to this client inside the VPN tunnel
Address = 10.8.0.2/32

# Internal DNS server IP (VLAN 210 AD/DNS) for internal resource resolution
DNS = 10.0.10.10 

[Peer]
# The Server's Public Key (allows the client to encrypt data to the server)
PublicKey = [SERVER_PUBLIC_KEY]

# The public IP and port of the WireGuard server 
Endpoint = 128.187.49.253:51820

# REQUIRED FOR SPLIT-TUNNELING:
# This lists all subnets the client is allowed to send through the tunnel.
# Includes: VPN Tunnel subnet (10.8.0.0/24) and all internal VLANs (210, 212, 213, 214)
AllowedIPs = 10.8.0.0/24, 10.0.10.0/24, 10.0.30.0/24, 10.0.40.0/24, 10.0.50.0/24

# Sends a small packet every 15 seconds to keep NAT open (recommended)
PersistentKeepalive = 15
```

### 4.2 Connection Steps

- Install Client: Install the WireGuard application. (https://www.wireguard.com/install/)

- Import Tunnel: Import the unique `client.conf` file.

  <img width="724" height="527" alt="image" src="https://github.com/user-attachments/assets/2600cf1d-898b-48a1-94d2-595b3bafb51f" />


- Activate Tunnel: Click Activate to establish the secure VPN tunnel.

  <img width="726" height="531" alt="image" src="https://github.com/user-attachments/assets/08a359b7-cfa3-48bb-9c6e-8d602515bd5c" />

- verify your new tunnel connection statistics.

  <img width="715" height="523" alt="image" src="https://github.com/user-attachments/assets/1f14a494-971c-49ab-99b1-37c5e236334c" />


## 5. ✅ Test Results and Troubleshooting Steps

Service Status

<img width="882" height="329" alt="image" src="https://github.com/user-attachments/assets/23bd2e16-18f7-48c7-b788-2c879fca8eee" />


Tunnel Check

<img width="528" height="128" alt="image" src="https://github.com/user-attachments/assets/15262d9f-1685-4e5f-addb-3a7109cba7af" />

Ping:

<img width="562" height="212" alt="image" src="https://github.com/user-attachments/assets/d8d20f14-833d-460f-bc83-9974bdc0d7ea" />

### Troubleshooting NAT:

If connection fails: Verify NAT and firewall rules on OPNsense are correct and the server's IP forwarding is enabled `sudo sysctl -p`

> No changes to GPO


