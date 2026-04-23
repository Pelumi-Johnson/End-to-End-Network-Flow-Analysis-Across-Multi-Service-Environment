# End to End Network Flow Analysis Across Multi Service Environment

## Overview
Designed and validated a complete end to end network communication flow integrating DNS resolution routing NAT and switching. Traced a full request lifecycle from hostname query to server response demonstrating how multiple network services operate together as a unified system.

## Objective
Implemented and analyzed full packet flow across the network to verify how DNS routing NAT and switching interact to deliver successful communication between a client and external server.

## Network Setup
Devices Used
1 Cisco 2960 Switch
1 Cisco 1941 Router
1 DNS Server
1 Web Server
1 End Device PC

Topology Design
PC connected to switch within local network
Switch connected to router using trunk or access depending on design
Router configured with internal and external interfaces for routing and NAT
DNS server configured to resolve domain names to IP addresses
Web server configured to respond to HTTP requests
Router connected to external network simulating ISP

Logical Flow
PC to Switch to Router to ISP to Server and back

Screenshot Placeholder Topology
![Network Topology](./screenshots/topology.png)

## Configuration

### DNS Configuration
Configured DNS server to resolve domain name to web server IP

Example
mysite.com mapped to 200.1.1.10

Screenshot Placeholder DNS Config
![DNS Config](./screenshots/dns-config.png)

### Routing and Gateway Configuration
Configured default gateway on PC and routing on router to forward traffic to external network

Screenshot Placeholder Routing Config
![Routing Config](./screenshots/routing-config.png)

### NAT Configuration
Configured NAT overload to translate private IP addresses to public interface

access-list 1 permit 192.168.10.0 0.0.0.255
ip nat inside source list 1 interface gigabitEthernet 0/1 overload

Screenshot Placeholder NAT Config
![NAT Config](./screenshots/nat-config.png)

### Server Configuration
Configured web server to respond to incoming requests

Screenshot Placeholder Web Server
![Web Server](./screenshots/web-server.png)

## End to End Flow Validation

### Step 1 DNS Resolution
Initiated request from PC

ping mysite.com

Observation
DNS server resolved domain name to IP address

Screenshot Placeholder DNS Resolution
![DNS Resolution](./screenshots/dns-resolution.png)

---

### Step 2 Packet Forwarding to Gateway
PC forwarded traffic to default gateway for external communication

Observation
Traffic left local subnet toward router

Screenshot Placeholder Gateway Forwarding
![Gateway Forwarding](./screenshots/gateway-forwarding.png)

---

### Step 3 NAT Translation
Router translated private IP to public IP before sending to external network

Example
192.168.10.10 translated to 200.1.1.1

Screenshot Placeholder NAT Translation
![NAT Translation](./screenshots/nat-translation.png)

---

### Step 4 External Routing
Traffic traversed external network toward destination server

Screenshot Placeholder External Routing
![External Routing](./screenshots/external-routing.png)

---

### Step 5 Server Response
Web server processed request and returned response

Screenshot Placeholder Server Response
![Server Response](./screenshots/server-response.png)

---

### Step 6 Reverse NAT Translation
Router translated response back to internal private IP

Example
200.1.1.1 translated back to 192.168.10.10

Screenshot Placeholder Reverse NAT
![Reverse NAT](./screenshots/reverse-nat.png)

---

### Step 7 Final Delivery
PC received response successfully completing full communication cycle

Screenshot Placeholder Final Response
![Final Response](./screenshots/final-response.png)

---

## Failure Testing

### DNS Failure
Disabled DNS resolution

Result
Domain name failed while direct IP communication remained functional

Screenshot Placeholder DNS Failure
![DNS Failure](./screenshots/dns-failure.png)

---

### NAT Failure
Removed NAT configuration

Result
Private IP could not reach external network

Screenshot Placeholder NAT Failure
![NAT Failure](./screenshots/nat-failure.png)

---

### Gateway Failure
Misconfigured default gateway

Result
Traffic failed to leave local network

Screenshot Placeholder Gateway Failure
![Gateway Failure](./screenshots/gateway-failure.png)

---

### VLAN or Switching Failure
Misconfigured VLAN assignment or switching path

Result
Local communication failed before reaching router

Screenshot Placeholder VLAN Failure
![VLAN Failure](./screenshots/vlan-failure.png)

---

## Key Concepts Applied
End to end packet flow analysis across multiple network layers
DNS resolution for domain to IP translation
Routing for path determination between networks
NAT for private to public address translation
Switching for local network delivery
Failure isolation based on identifying break point in communication path
System level understanding of network behavior rather than isolated configurations

## Outcome
Validated complete network communication lifecycle across DNS routing NAT and switching. Demonstrated how each component contributes to successful data delivery and how failure in any layer disrupts the entire process. Established a system level perspective required for real world network troubleshooting and design.
