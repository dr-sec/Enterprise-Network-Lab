# Lab-02---VLAN-Segmentation-Across-Two-Switches

## Objective

Build a small enterprise-style network using Cisco Packet Tracer to understand how VLANs segment network traffic between departments while allowing communication between devices in the same department across multiple switches.

---

## Network Topology

```
                Trunk Link
           --------------------
           |                  |
      Floor 1 Switch     Floor 2 Switch
           |                  |
      Sales PC           Sales PC
      VLAN 10            VLAN 10

      Marketing PC       Marketing PC
      VLAN 20            VLAN 20
```

---

## VLAN Configuration

| VLAN | Department | Network |
|------|------------|----------------|
| 10 | Sales | 192.168.10.0/24 |
| 20 | Marketing | 192.168.20.0/24 |

---

## Switch Configuration

- Created VLAN 10 (Sales)
- Created VLAN 20 (Marketing)
- Configured PC ports as Access Ports
- Configured the link between switches as an 802.1Q Trunk
- Allowed VLANs 10 and 20 across the trunk

---

## IP Addressing

### Sales

- PC0 - 192.168.10.10
- PC3 - 192.168.10.11

### Marketing

- PC1 - 192.168.20.10
- PC2 - 192.168.20.11

---

## Verification

### Successful

- Sales PC ↔ Sales PC
- Marketing PC ↔ Marketing PC

### Failed (Expected)

- Sales PC ↔ Marketing PC

---

## What I Learned

- VLANs logically separate devices into different broadcast domains.
- Devices within the same VLAN can communicate even when connected to different switches.
- Trunk ports carry traffic for multiple VLANs between switches.
- Access ports assign a single VLAN to an endpoint device.
- Devices in different VLANs require Layer 3 routing before they can communicate.

---

## Skills Practiced

- Cisco Packet Tracer
- VLAN Configuration
- Access Ports
- Trunk Ports
- IPv4 Addressing
- Network Segmentation
- Connectivity Testing
