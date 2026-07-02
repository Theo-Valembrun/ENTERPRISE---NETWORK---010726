# Enterprise Network - 010726

<p align="center">
  <img src="Enterprise%20-%20Network%20-%20010726.png" alt="Enterprise Network topology" width="1400" />
</p>

## Project Summary

This repository documents a Cisco Packet Tracer enterprise network built around a 3-tier design: access, distribution, and core. The lab includes VLAN segmentation, inter-VLAN routing, DHCP relay on the switch layer, centralized logging, server services, and an ASA firewall separating the internal network from the edge router.

## Key Design Elements

- Hierarchical 3-layer architecture
- VLAN 10 for servers, VLAN 20 for Sales, VLAN 30 for HR, VLAN 40 for IT, VLAN 50 for Finance, and VLAN 99 for management
- 802.1Q trunks between access, distribution, and core layers
- Layer 3 routing on the core switch through SVIs
- Static routing across the core, firewall, and edge router
- ASA 5506-X stateful firewall with inside and outside zones
- DHCP relay on the switches and centralized syslog forwarding

## Addressing Plan

| Segment | Network | Gateway |
| --- | --- | --- |
| VLAN 10 - Server Farm | 10.0.10.0/24 | 10.0.10.1 |
| VLAN 20 - Sales | 10.0.20.0/24 | 10.0.20.1 |
| VLAN 30 - HR | 10.0.30.0/24 | 10.0.30.1 |
| VLAN 40 - IT | 10.0.40.0/24 | 10.0.40.1 |
| VLAN 50 - Finance | 10.0.50.0/24 | 10.0.50.1 |
| VLAN 99 - Management | 10.0.99.0/24 | 10.0.99.1 |
| Core to Firewall | 10.0.0.0/30 | Core: 10.0.0.1 / Firewall: 10.0.0.2 |
| Firewall to Edge Router | 203.0.113.4/30 | Firewall: 203.0.113.5 / Edge: 203.0.113.6 |

## Services

- Web and DNS services hosted in the server farm
- DHCP relay configured on the switches, with the DHCP service hosted on the server farm
- Syslog logs forwarded from the core to the DB server
- Controlled ICMP inspection on the ASA for stateful connectivity testing

## Repository Contents

- `Enterprise - Network - 010726.pkt` - Packet Tracer project file
- `Enterprise - Network - 010726.png` - Network topology diagram
- `*_running-config.txt` and `*_startup-config.txt` - Device configuration exports

## Presentation Notes

The repository is structured to show the final validated state of the network lab. The Packet Tracer project can be reopened for further iteration if required.