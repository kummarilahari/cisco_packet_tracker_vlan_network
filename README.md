# Secure Office Network Using VLANs — Cisco Packet Tracer

A small office network with three department VLANs (HR, IT, Finance), two Cisco 2960 switches, and Router-on-a-Stick inter-VLAN routing.

> **Important:** This GitHub repository contains configuration files, documentation, and a topology illustration. It does **not** contain a native Cisco Packet Tracer `.pkt` file or genuine Packet Tracer screenshots. Build and test the topology in Packet Tracer, then add your own `.pkt` file and screenshots.

## Topology

![Office VLAN topology](images/office-vlan-topology.png)

## VLAN and addressing plan

| VLAN | Department | Network | Default gateway |
|---|---|---|---|
| 10 | HR | `192.168.10.0/24` | `192.168.10.1` |
| 20 | IT | `192.168.20.0/24` | `192.168.20.1` |
| 30 | Finance | `192.168.30.0/24` | `192.168.30.1` |

| PC | Switch port | IP address | Gateway |
|---|---|---|---|
| HR-PC1 | Switch1 Fa0/1 | `192.168.10.11/24` | `192.168.10.1` |
| IT-PC1 | Switch1 Fa0/2 | `192.168.20.11/24` | `192.168.20.1` |
| FIN-PC1 | Switch1 Fa0/3 | `192.168.30.11/24` | `192.168.30.1` |
| HR-PC2 | Switch2 Fa0/1 | `192.168.10.12/24` | `192.168.10.1` |
| IT-PC2 | Switch2 Fa0/2 | `192.168.20.12/24` | `192.168.20.1` |
| FIN-PC2 | Switch2 Fa0/3 | `192.168.30.12/24` | `192.168.30.1` |

Subnet mask for all PCs: `255.255.255.0`

## Cabling

- Office-Router `G0/0` ↔ Switch1 `G0/1` (802.1Q trunk)
- Switch1 `G0/2` ↔ Switch2 `G0/1` (802.1Q trunk)
- Switch1 access ports: Fa0/1 HR-PC1, Fa0/2 IT-PC1, Fa0/3 FIN-PC1
- Switch2 access ports: Fa0/1 HR-PC2, Fa0/2 IT-PC2, Fa0/3 FIN-PC2

Use a router with an Ethernet interface that supports 802.1Q subinterfaces. If your router's interface name differs, adjust the commands accordingly.

## Configure in Packet Tracer

1. Add one compatible router, two 2960 switches, and six PCs.
2. Connect the devices using the cabling table above.
3. Enter `configs/switch1.txt` on Switch1.
4. Enter `configs/switch2.txt` on Switch2.
5. Enter `configs/router.txt` on the router.
6. Set each PC's static IP address and gateway using the table above.
7. Verify VLANs, trunks, interfaces, and routing with the commands below.
8. Test same-VLAN and inter-VLAN pings.
9. Save the actual Packet Tracer project as `StudentName_VLAN_Routing_Project.pkt`.
10. Capture genuine screenshots from your Packet Tracer application and put them in `screenshots/`.

## Verification commands

### Switches
```text
show vlan brief
show interfaces trunk
show interfaces status
show mac address-table
show running-config
```

### Router
```text
show ip interface brief
show ip route
show running-config
show interfaces
```

## Ping tests

From HR-PC1's Desktop > Command Prompt:
```text
ping 192.168.10.12
ping 192.168.20.11
ping 192.168.30.11
```

Also test from IT-PC1 to `192.168.20.12`, and from FIN-PC1 to `192.168.30.12`. Repeat a ping if the first attempt times out during ARP resolution.

## Simulation Mode notes

- HR-PC1 uses ARP to discover the MAC address for its default gateway, `192.168.10.1`.
- The switch forwards HR traffic over the VLAN 10 trunk to the router.
- The router routes the packet using its routing table and sends it out subinterface `G0/0.30` for the Finance network.
- The router builds a new Ethernet frame on the outgoing network, so the Ethernet source and destination MAC addresses change.
- Without NAT, the source and destination IP addresses remain unchanged; TTL and the IP header checksum are updated by the router.

## Troubleshooting

| Symptom | Check |
|---|---|
| Same-VLAN ping fails | PC IP/mask, access VLAN, cable, switch port status |
| Cross-VLAN ping fails | PC gateway, router subinterfaces, dot1Q VLAN IDs, trunks |
| Trunk not shown | Physical connection, correct interface, `switchport mode trunk` |
| Router interface down | Parent interface and cable; run `no shutdown` |
| Finance gateway unreachable | Ensure Finance gateway is `192.168.30.1` |

## Security note

VLANs provide logical separation and reduce broadcast domains, but VLANs alone are not a complete security boundary. Use access-control lists and other appropriate controls if departments must be prevented from accessing each other's systems.

## Suggested GitHub screenshots

After you build and test the network, add these real screenshots:
- `screenshots/01-topology.png` — full Packet Tracer topology
- `screenshots/02-switch1-vlans.png` — `show vlan brief`
- `screenshots/03-trunks.png` — `show interfaces trunk`
- `screenshots/04-router-interfaces.png` — `show ip interface brief`
- `screenshots/05-same-vlan-ping.png` — successful same-VLAN ping
- `screenshots/06-inter-vlan-ping.png` — successful inter-VLAN ping
- `screenshots/07-simulation.png` — Simulation Mode packet flow

Do not label the included topology illustration as a Packet Tracer screenshot; it is a documentation diagram.
