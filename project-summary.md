# Project Summary

## Objective
Separate HR, IT, and Finance into VLANs and enable inter-VLAN communication through Router-on-a-Stick.

## Configuration
- VLAN 10: HR — 192.168.10.0/24
- VLAN 20: IT — 192.168.20.0/24
- VLAN 30: Finance — 192.168.30.0/24
- Router gateways: 192.168.10.1, 192.168.20.1, 192.168.30.1

## Conclusion
VLANs organize the network into separate broadcast domains. Trunk links carry multiple VLANs between switches and the router, and router subinterfaces enable inter-VLAN routing. VLANs alone do not enforce strict department access restrictions; ACLs or other controls may be needed.
