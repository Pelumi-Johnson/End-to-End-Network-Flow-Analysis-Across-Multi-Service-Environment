# End to End Network Flow Analysis Across Multi Service Environment

## Overview
Designed and validated a complete end to end network communication flow integrating VLAN segmentation DNS resolution routing NAT and switching. Traced a full request lifecycle from hostname query to server response demonstrating how multiple network services operate together as a unified system.

## Objective
Implemented and analyzed full packet flow across segmented VLAN networks to verify how DNS routing NAT and switching interact to deliver successful communication between internal hosts and external server.
```
## Network Setup
Devices Used
1 Cisco 2960 Switch
1 Cisco 1941 Router
1 DNS Server
1 Web Server
2 End Devices PCs
```
Topology Design
- PC0 assigned to VLAN 10 subnet 192.168.1.0
- PC1 assigned to VLAN 20 subnet 192.168.2.0
- Switch configured with VLAN segmentation
- Router configured using Router on a Stick for inter VLAN routing
- Router connected to external network simulating ISP
- DNS server configured for domain resolution
- Web server configured to respond to requests

Logical Flow
PC to Switch to Router to ISP to Server and back

Screenshot Placeholder Topology
![Network Topology](./screenshots/topology.png)

## Configuration

### VLAN Configuration

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

Screenshot Placeholder VLAN Config
![VLAN Config](./screenshots/vlan-config.png)

---

### Router on a Stick Configuration

interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.1.1 255.255.255.0

interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.2.1 255.255.255.0

interface gigabitEthernet 0/0
no shutdown

Screenshot Placeholder Router VLAN Config
![Router VLAN Config](./screenshots/router-vlan-config.png)

---

### DNS Configuration
Configured DNS server to resolve domain name to web server IP

Example
mysite.com mapped to 200.1.1.10

Screenshot Placeholder DNS Config
![DNS Config](./screenshots/dns-config.png)

---

### NAT Configuration

access-list 1 permit 192.168.1.0 0.0.0.255
access-list 1 permit 192.168.2.0 0.0.0.255

interface gigabitEthernet 0/0
ip nat inside

interface gigabitEthernet 0/1
ip nat outside

ip nat inside source list 1 interface gigabitEthernet 0/1 overload

Screenshot Placeholder NAT Config
![NAT Config](./screenshots/nat-config.png)

---

### End Device Configuration

PC0 VLAN 10
IP 192.168.1.10
Subnet Mask 255.255.255.0
Default Gateway 192.168.1.1

PC1 VLAN 20
IP 192.168.2.10
Subnet Mask 255.255.255.0
Default Gateway 192.168.2.1

Screenshot Placeholder PC Config
![PC Config](./screenshots/pc-config.png)

---

## End to End Flow Validation

### Step 1 DNS Resolution

ping mysite.com

Observation
Domain name resolved to IP address through DNS

Screenshot Placeholder DNS Resolution
![DNS Resolution](./screenshots/dns-resolution.png)

---

### Step 2 Inter VLAN Routing

Traffic from VLAN 10 routed through router to reach external network

Observation
Router handled communication between VLANs and forwarded traffic

Screenshot Placeholder Inter VLAN Routing
![Inter VLAN Routing](./screenshots/inter-vlan.png)

---

### Step 3 NAT Translation

Private IP translated to public IP before leaving network

Example
192.168.1.10 translated to 200.1.1.1

Screenshot Placeholder NAT Translation
![NAT Translation](./screenshots/nat-translation.png)

---

### Step 4 External Communication

Traffic reached external server and response returned

Screenshot Placeholder External Communication
![External Communication](./screenshots/external.png)

---

### Step 5 Reverse NAT and Final Delivery

Router translated response back to internal IP and delivered to correct VLAN host

Screenshot Placeholder Final Delivery
![Final Delivery](./screenshots/final-delivery.png)

---

## Failure Testing

### DNS Failure
Name resolution failed while direct IP communication remained functional

---

### NAT Failure
Private IP unable to reach external network

---

### Gateway Failure
Traffic could not leave local VLAN

---

### VLAN Misconfiguration
Device unable to communicate within or across VLANs

---

## Key Concepts Applied
End to end packet flow across VLAN segmented networks
Router on a Stick for inter VLAN communication
DNS resolution for domain translation
NAT overload for private to public translation
Switching for local VLAN delivery
Systematic troubleshooting based on flow analysis

## Outcome
Validated full communication lifecycle across segmented VLAN environment integrating DNS routing NAT and switching. Demonstrated how traffic moves across multiple layers and how each component contributes to successful delivery. Established a system level understanding required for real world network design and troubleshooting.
