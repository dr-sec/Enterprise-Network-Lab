# Enterprise Network Lab

## Overview

This lab is a full Cisco Packet Tracer enterprise network designed to demonstrate how multiple enterprise networking technologies work together in a single topology.

The network includes multiple VLANs, Layer 3 switching, redundant gateways using HSRP, OSPF routing, 802.1Q trunking, and a WAN connection between network segments.

The goal was to build and verify an enterprise-style network rather than simply configure individual technologies in isolation.

---

## Technologies Demonstrated

- Enterprise network design
- Cisco Packet Tracer topology design
- VLAN configuration
- Access port configuration
- 802.1Q trunking
- Inter-VLAN routing using SVIs
- Layer 3 switching
- HSRP (Hot Standby Router Protocol)
- OSPF routing
- WAN configuration
- Redundant default gateways
- End-to-end connectivity testing
- Basic network resiliency/failover testing

---

## Network Design

The enterprise network is divided into multiple VLANs:

| VLAN | Department |
|---|---|
| VLAN 10 | HR |
| VLAN 20 | Sales |
| VLAN 30 | Finance |

The VLANs are carried between switches using 802.1Q trunks.

Layer 3 switches provide the VLAN gateway interfaces (SVIs), allowing hosts in different VLANs to communicate.

The network also contains a WAN/routed portion using OSPF to exchange routes between Layer 3 devices.

---

## VLAN Configuration

The following VLANs were created:

```text
VLAN 10 - HR
VLAN 20 - SALES
VLAN 30 - FINANCE
```

Access ports were assigned to the appropriate VLANs for end-user devices.

Trunk links were configured to carry VLANs 10, 20, and 30 between network devices.

Example verification:

```cisco
show vlan brief
show interfaces trunk
```

### Trunk Verification

The completed network showed the following VLANs being carried across the trunk links:

```text
Port        Vlans allowed
Fa0/1       10,20,30
Fa0/2       10,20,30
Fa0/3       10,20,30
Gig0/2      10,20,30
```

This verified that the required VLANs were being transported across the trunk connections.

---

# Layer 3 Switching and SVIs

The multilayer switches were configured to perform routing between VLANs using Switched Virtual Interfaces (SVIs).

The directly connected VLAN networks included:

```text
192.168.10.0/24
192.168.20.0/24
192.168.30.0/24
```

Verification with:

```cisco
show ip route
```

showed the VLAN networks as directly connected through their respective SVIs.

---

# HSRP

HSRP was implemented to provide redundant default gateways for the VLANs.

The HSRP virtual gateway addresses were:

| VLAN | Virtual IP |
|---|---|
| VLAN 10 | 192.168.10.1 |
| VLAN 20 | 192.168.20.1 |
| VLAN 30 | 192.168.30.1 |

The primary multilayer switch was configured with a priority of 110 and became the active HSRP router.

Example verification:

```cisco
show standby
show standby brief
```

Example output:

```text
Interface   Grp  Pri   State    Active   Standby          Virtual IP
Vl10        10   110   Active   local    192.168.10.3     192.168.10.1
Vl20        20   110   Active   local    192.168.20.3     192.168.20.1
Vl30        30   110   Active   local    192.168.30.3     192.168.30.1
```

This demonstrates that the switch was acting as the active gateway while another Layer 3 switch was available as the standby gateway.

---

# OSPF Routing

OSPF was used to dynamically exchange routes between the Layer 3 portion of the enterprise network and the routed/WAN portion.

OSPF neighbor relationships were verified with:

```cisco
show ip ospf neighbor
```

Example:

```text
Neighbor ID     Pri   State        Address       Interface
10.10.10.1      1     FULL/BDR     10.10.1.1     GigabitEthernet0/1
```

The routing table also showed OSPF-learned routes:

```text
O  10.10.1.4/30
O  10.10.2.0/30
O  10.10.2.4/30
O  10.10.10.0/30
O  192.168.40.0/24
O  192.168.50.0/24
O  192.168.60.0/24
```

This confirmed that the network was successfully learning remote routes dynamically through OSPF.

---

# WAN Configuration

The two major portions of the enterprise topology were connected through a routed WAN link.

The WAN portion used Layer 3 addressing and OSPF to provide connectivity between the network segments.

This allowed hosts on opposite sides of the enterprise topology to communicate without requiring a single Layer 2 broadcast domain.

---

# Connectivity Testing

End-to-end connectivity was tested between hosts on different VLANs and across different portions of the network.

Example:

```text
ping 192.168.40.10
ping 192.168.60.10
```

The tests successfully returned replies, demonstrating that:

- VLAN segmentation was functioning
- Inter-VLAN routing was functioning
- Layer 3 switching was functioning
- OSPF was providing routes to remote networks
- The WAN connection was functioning
- End-to-end connectivity was established

The first ping in some tests timed out while subsequent packets succeeded. This is expected behavior in a simulated network because devices may need to resolve ARP information before forwarding traffic.

---

# Redundancy / Failover

HSRP was used to provide gateway redundancy.

The network was also designed with redundant Layer 3 paths so that connectivity could continue if one path or gateway became unavailable.

As part of the lab, the multilayer switch failover behavior was tested conceptually/through the topology after verifying that HSRP had established an active and standby relationship.

The purpose of this was to understand how enterprise networks avoid relying on a single default gateway.

---

# Verification Commands

The following Cisco commands were used to verify the configuration:

### VLANs

```cisco
show vlan brief
```

### Trunking

```cisco
show interfaces trunk
```

### HSRP

```cisco
show standby
show standby brief
```

### OSPF Neighbors

```cisco
show ip ospf neighbor
```

### Routing Table

```cisco
show ip route
```

### Interface Status

```cisco
show ip interface brief
```

---

# What I Learned

This lab tied together the concepts from my previous Cisco Packet Tracer labs into one larger enterprise topology.

The most important concepts reinforced were:

1. **VLANs** provide logical segmentation of a network.
2. **Access ports** connect end devices to a specific VLAN.
3. **802.1Q trunks** allow multiple VLANs to travel between switches.
4. **SVIs** allow multilayer switches to route between VLANs.
5. **HSRP** provides a redundant default gateway using a virtual IP address.
6. **Layer 3 switches** can perform routing without requiring a separate router for every VLAN.
7. **OSPF** dynamically learns routes between routers and Layer 3 switches.
8. **WAN links** allow separate networks to communicate across routed infrastructure.
9. **Redundancy** is important in enterprise networks because a single failed device should not necessarily bring down connectivity.
10. **Verification commands** such as `show ip route`, `show standby`, and `show interfaces trunk` are essential for troubleshooting and proving that a configuration is working.

---

# Additional Features

DHCP, DNS, ACLs, and other security/services were not duplicated in this lab because they were already implemented and documented in separate Packet Tracer labs.

This project focuses on bringing the core switching, routing, VLAN, redundancy, and WAN concepts together into one enterprise topology.

---

## Project File

The Cisco Packet Tracer project file is included in this repository:

```text
Enterprise Network.pkt
```

Open the `.pkt` file in Cisco Packet Tracer to inspect the complete topology and configurations.

