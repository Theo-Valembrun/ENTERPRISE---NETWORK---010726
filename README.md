# Enterprise Network - 010726

<p align="center">
  <img src="Enterprise%20-%20Network%20-%20010726.png" alt="Enterprise Network topology" width="1400" />
</p>

## Overview

This Packet Tracer project models a clean 3-tier enterprise network with access, distribution, and core layers. It includes VLAN segmentation, inter-VLAN routing, DHCP relay, DNS, HTTP, logging, and an ASA firewall protecting the internal network from the outside edge.

## Topology Highlights

- 3-layer design: Access, Distribution, Core
- VLANs: 10 Servers, 20 Sales, 30 HR, 40 IT, 50 Finance, 99 Management
- Trunk links using 802.1Q between layers
- Inter-VLAN routing on the Layer 3 core switch
- Static routing from Core to Firewall to Edge Router
- ASA 5506-X stateful inspection with inside/outside zones
- DHCP relay and centralized syslog logging

## Addressing Plan

| Segment | Network | Gateway |
| --- | --- | --- |
| VLAN 10 - Server Farm | 10.0.10.0/24 | 10.0.10.1 |
| VLAN 20 - Sales | 10.0.20.0/24 | 10.0.20.1 |
| VLAN 30 - HR | 10.0.30.0/24 | 10.0.30.1 |
| VLAN 40 - IT | 10.0.40.0/24 | 10.0.40.1 |
| VLAN 50 - Finance | 10.0.50.0/24 | 10.0.50.1 |
| VLAN 99 - Management | 10.0.99.0/24 | 10.0.99.1 |
| Core - Firewall | 10.0.0.0/30 | Core: 10.0.0.1 / Firewall: 10.0.0.2 |
| Firewall - Edge Router | 203.0.113.4/30 | Firewall: 203.0.113.5 / Edge: 203.0.113.6 |

## Services

- Web and DNS services on the server farm
- DHCP for client subnets with relay on the core switch
- Syslog forwarding from the core to the DB server
- ICMP inspection on the ASA for controlled outbound and return traffic

## Files Included

- `Enterprise - Network - 010726.pkt` - Packet Tracer project
- `Enterprise - Network - 010726.png` - Network diagram
- `*_running-config.txt` / `*_startup-config.txt` - Device configurations

## Notes

This repository is meant to document the final working state of the lab. If you want to continue iterating, open the `.pkt` file in Cisco Packet Tracer and adjust the configs from there.