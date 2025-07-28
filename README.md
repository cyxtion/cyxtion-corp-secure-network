# Cyxtion Corp Secure Network Simulation (Cisco Packet Tracer)

This project simulates a secure, segmented corporate network for **Cyxtion Corp** using Cisco Packet Tracer. It features VLAN-based department segmentation, inter-VLAN routing using Router-on-a-Stick and is part of a broader secure architecture design including DMZ and ACL implementation.

---

## Project Overview

**Goal:**

> Build a scalable, secure enterprise network topology with inter-departmental segmentation and controlled access using best practices in networking and security fundamentals.

**Core Features Implemented in Phase 1:**

* Department-wise VLAN segmentation
* Inter-VLAN routing via sub-interfaces
* Trunking between switch and router
* Unique IP scheme for each department
* Full device-to-device communication across VLANs

---

## Topology Components

### End Devices

* HR-PC
* IT-PC
* Admin-PC
* Web Server (DMZ)
* DNS Server (DMZ)
* Internet-PC 1 & 2

### Networking Devices

* 1x Cisco 2911 Router
* 3x Cisco 2960 Switches

  * Internal VLAN Switch
  * DMZ Switch
  * External Switch

---

## IP Addressing Scheme

| VLAN ID | Department | Subnet           | Gateway       |
| ------- | ---------- | ---------------- | ------------- |
| 10      | HR         | 192.168.10.0/24  | 192.168.10.1  |
| 20      | IT         | 192.168.20.0/24  | 192.168.20.1  |
| 30      | Admin      | 192.168.30.0/24  | 192.168.30.1  |
| 100     | DMZ        | 192.168.100.0/24 | 192.168.100.1 |
| ---     | External   | 200.0.0.0/24     | 200.0.0.1     |

---

## VLAN & Port Configuration

### VLAN Creation

```bash
vlan 10
 name HR
vlan 20
 name IT
vlan 30
 name Admin
vlan 100
 name DMZ
```

### Assigning Access Ports

```bash
interface range fa0/1
 switchport mode access
 switchport access vlan 10
exit

interface range fa0/2
 switchport mode access
 switchport access vlan 20
exit

interface range fa0/3
 switchport mode access
 switchport access vlan 30
exit
```

### Trunk Port (To Router)

```bash
interface fa0/4
 switchport mode trunk
```

---

## Router Configuration (Router-on-a-Stick)

### Sub-Interfaces

```bash
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0

interface GigabitEthernet0/1
 ip address 192.168.100.1 255.255.255.0

interface GigabitEthernet0/2
 ip address 200.0.0.1 255.255.255.0
```

---

## Device IP Assignments

### HR-PC

* IP: `192.168.10.10`
* Gateway: `192.168.10.1`

### IT-PC

* IP: `192.168.20.10`
* Gateway: `192.168.20.1`

### Admin-PC

* IP: `192.168.30.10`
* Gateway: `192.168.30.1`

### Internet-PCs

* Internet-PC1: `200.0.0.10`, Gateway: `200.0.0.1`
* Internet-PC2: `200.0.0.11`, Gateway: `200.0.0.1`

### Web & DNS Servers (DMZ)

* Web Server: `192.168.100.10`
* DNS Server: `192.168.100.20`

---

## Connectivity Testing

### All VLAN PCs Can:

* Ping their own gateway
* Ping each other
* Ping the router

### Example Ping Results:

```bash
HR-PC> ping 192.168.20.10
Reply from 192.168.20.10: bytes=32 time<1ms TTL=128

Admin-PC> ping 192.168.10.10
Reply from 192.168.10.10: bytes=32 time<1ms TTL=128
```

---

## Phase 2: DMZ Setup + ACLs

### ACL Configuration

```bash
ip access-list extended 100
 permit ip 192.168.30.0 0.0.0.255 192.168.100.0 0.0.0.255
 deny ip 192.168.10.0 0.0.0.255 192.168.100.0 0.0.0.255
 deny ip 192.168.20.0 0.0.0.255 192.168.100.0 0.0.0.255
 permit ip 200.0.0.0 0.0.0.255 192.168.100.0 0.0.0.255
 deny ip 200.0.0.0 0.0.0.255 192.168.10.0 0.0.0.255
 deny ip 200.0.0.0 0.0.0.255 192.168.20.0 0.0.0.255
 deny ip 200.0.0.0 0.0.0.255 192.168.30.0 0.0.0.255
 permit ip any any
```

### Applied to:

```bash
interface GigabitEthernet0/1
 ip access-group 100 in
```

### Important Note:

> Cisco Packet Tracer may not enforce ICMP (ping) blocking correctly in complex ACL scenarios. Although ping tests from blocked VLANs or Internet PCs may still succeed, ACL rules are valid and would function as intended on real Cisco IOS devices.

---

## Screenshots

*(All screenshots located in the `/screenshots` directory)*

* `topology-overview.png`
* `vlan-brief.png`
* `switchport-fa01-fa04.png`
* `router-ip-interface.png`
* `router-running-config.png`
* `ping-tests-hr.png`
* `ping-tests-it.png`
* `ping-tests-admin.png`
* `hr-ip-config.png`
* `it-ip-config.png`
* `admin-ip-config.png`
* `web-server-ip-config.png`
* `web-server-ping-tests.png`
* `mail-server-ip-config.png`
* `mail-server-ping-tests.png`
* `router-ip-interface-brief.png`
* `acl-show-access-lists.png`
* `acl-interface-binding.png`

---

## Author

Made with CLI by **Cyxtion**

*For more info, contact via GitHub or LinkedIn. Stay tuned for future upgrades like NAT, DHCP or VPN!*
