# End to End Network Flow Analysis Across Multi Service Environment

## Overview
Designed and validated a complete end to end network communication flow integrating VLAN segmentation DHCP DNS routing NAT and switching. Traced a full request lifecycle from hostname query to server response demonstrating how multiple network services operate together as a unified system.

## Objective
Implemented and analyzed full packet flow across segmented VLAN networks to verify how DHCP DNS routing NAT and switching interact to deliver successful communication between internal hosts and external server.
```
## Network Setup
Devices Used
1 Cisco 2960 Switch
2 Cisco 1941 Routers
1 DNS Server
1 Web Server
2 End Devices PCs
```
Topology Design
- PC0 assigned to VLAN 10 subnet 192.168.1.0
- PC1 assigned to VLAN 20 subnet 192.168.2.0
- Switch configured with VLAN segmentation
- Router0 configured using Router on a Stick for inter VLAN routing DHCP and NAT
- Router1 configured to simulate ISP side routing
- DNS server configured for domain resolution
- Web server configured to respond to requests

Logical Flow
PC to Switch to Router0 to Router1 to Server and back

Topology
![Network Topology](https://github.com/Pelumi-Johnson/End-to-End-Network-Flow-Analysis-Across-Multi-Service-Environment/blob/main/Screenshot%202026-04-23%20010420.png)

## Configuration

### VLAN Configuration
```
enable
configure terminal

vlan 10
name HR

vlan 20
name IT

interface fastEthernet 0/1
switchport mode access
switchport access vlan 10

interface fastEthernet 0/2
switchport mode access
switchport access vlan 20

interface fastEthernet 0/24
switchport mode trunk
exit
```
VLAN Config
![VLAN Config](https://github.com/Pelumi-Johnson/End-to-End-Network-Flow-Analysis-Across-Multi-Service-Environment/blob/main/Screenshot%202026-04-23%20011023.png)

---

### Router0 Inter VLAN Routing Configuration
```
interface gigabitEthernet 0/0
no shutdown

interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.1.1 255.255.255.0

interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.2.1 255.255.255.0
```
Router0 VLAN Config
![Router0 VLAN Config](https://github.com/Pelumi-Johnson/End-to-End-Network-Flow-Analysis-Across-Multi-Service-Environment/blob/main/Screenshot%202026-04-23%20011348.png)

---

### Router0 DHCP Configuration
```
ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp excluded-address 192.168.2.1 192.168.2.10

ip dhcp pool HR
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
dns-server 200.1.1.10

ip dhcp pool IT
network 192.168.2.0 255.255.255.0
default-router 192.168.2.1
dns-server 200.1.1.10
```
DHCP Config
![DHCP Config](https://github.com/Pelumi-Johnson/End-to-End-Network-Flow-Analysis-Across-Multi-Service-Environment/blob/main/Screenshot%202026-04-23%20012450.png)

---

### Router0 NAT and Outside Interface Configuration
```
interface gigabitEthernet 0/1
ip address 200.1.1.1 255.255.255.0
ip nat outside
no shutdown

interface gigabitEthernet 0/0
ip nat inside

access-list 1 permit 192.168.1.0 0.0.0.255
access-list 1 permit 192.168.2.0 0.0.0.255

ip nat inside source list 1 interface gigabitEthernet 0/1 overload
```
Router0 NAT Config
![Router0 NAT Config](https://github.com/Pelumi-Johnson/End-to-End-Network-Flow-Analysis-Across-Multi-Service-Environment/blob/main/Screenshot%202026-04-23%20012803.png)

---

### Router1 ISP Side Configuration
```
interface gigabitEthernet 0/0
ip address 200.1.1.2 255.255.255.0
no shutdown

interface gigabitEthernet 0/1
ip address 200.1.2.1 255.255.255.0
no shutdown

```
Router1 Config
![Router1 Config](https://github.com/Pelumi-Johnson/End-to-End-Network-Flow-Analysis-Across-Multi-Service-Environment/blob/main/Screenshot%202026-04-23%20013258.png)

---

### DNS and Web Server Configuration
```
DNS Server
IP Address 200.1.2.10
Subnet Mask 255.255.255.0
Default Gateway 200.1.2.1
```
DNS Record
pelumijohnson.com mapped to 200.1.2.10
```
Web Server
IP Address 200.1.2.10
Subnet Mask 255.255.255.0
Default Gateway 200.1.2.1
```
Server Config
![Server Config](https://github.com/Pelumi-Johnson/End-to-End-Network-Flow-Analysis-Across-Multi-Service-Environment/blob/main/Screenshot%202026-04-23%20013702.png)
---
![Server Config](https://github.com/Pelumi-Johnson/End-to-End-Network-Flow-Analysis-Across-Multi-Service-Environment/blob/main/Screenshot%202026-04-23%20013713.png)

---

### End Device Configuration
```
PC0 VLAN 10
Configured to receive IP address dynamically from DHCP

PC1 VLAN 20
Configured to receive IP address dynamically from DHCP

Example Assigned Addresses
PC0 192.168.1.11
PC1 192.168.2.11
```
PC Config
![PC Config](https://github.com/Pelumi-Johnson/End-to-End-Network-Flow-Analysis-Across-Multi-Service-Environment/blob/main/Screenshot%202026-04-23%20013816.png)
---
![PC Config](https://github.com/Pelumi-Johnson/End-to-End-Network-Flow-Analysis-Across-Multi-Service-Environment/blob/main/Screenshot%202026-04-23%20013844.png)

---

### Step 2 DNS Resolution
```
ping pelumijohnson.com
```
Observation
Domain name resolved to web server IP through DNS

DNS Resolution
![DNS Resolution](https://github.com/Pelumi-Johnson/End-to-End-Network-Flow-Analysis-Across-Multi-Service-Environment/blob/main/Screenshot%202026-04-23%20015004.png)

---

### Step 3 Inter VLAN and Gateway Forwarding

Traffic from internal VLAN host forwarded to Router0 for external communication

Observation
Router0 processed gateway decision and prepared packet for outbound translation

Gateway Forwarding
![Gateway Forwarding](https://github.com/Pelumi-Johnson/End-to-End-Network-Flow-Analysis-Across-Multi-Service-Environment/blob/main/Animation.gif)

---

## Key Concepts Applied
- VLAN segmentation across 192.168.1.0 and 192.168.2.0 networks
- Router on a Stick for inter VLAN communication
- DHCP service delivery from Router0 to multiple VLANs
- NAT overload for private to public translation
- ISP side routing with Router1 across external networks
- DNS resolution for domain translation
- Systematic end to end flow analysis

## Outcome
Validated full communication lifecycle across segmented VLAN environment integrating DHCP DNS routing NAT and switching. Demonstrated how internal hosts obtained addressing automatically, resolved external resources by name, traversed translated paths through Router0 and Router1, and successfully reached external services. Established a complete multi service topology reflecting real world enterprise traffic flow.
