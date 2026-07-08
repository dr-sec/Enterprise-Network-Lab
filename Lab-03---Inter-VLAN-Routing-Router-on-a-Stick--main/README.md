# Lab 03 - Inter-VLAN Routing (Router-on-a-Stick)

## Objective

Build a routed network using Cisco Packet Tracer that enables communication between two separate VLANs through a Cisco router using Router-on-a-Stick.

---

## Skills Learned

- VLAN Routing
- Router-on-a-Stick
- 802.1Q Trunking
- Router Subinterfaces
- Default Gateways
- ARP
- Inter-VLAN Communication
- Connectivity Testing

---

## Network Topology

```
                    Cisco 2911 Router
                  +-------------------+
                  |                   |
             Gi0/0                   Gi0/1
                |                       |
         VLAN 10 Switch          VLAN 20 Switch
                |                       |
        PC0         PC1         PC2         PC3
```

---

## IP Addressing

### VLAN 10 - Department A

| Device | IP Address |
|---------|------------|
| PC0 | 192.168.10.11 |
| PC1 | 192.168.10.12 |
| Gateway | 192.168.10.1 |

### VLAN 20 - Department B

| Device | IP Address |
|---------|------------|
| PC2 | 192.168.20.13 |
| PC3 | 192.168.20.14 |
| Gateway | 192.168.20.1 |

---

## Configuration Summary

### Switches

- Configured access ports for end devices.
- Assigned ports to their respective VLANs.
- Connected each switch to the router.

### Router

- Configured GigabitEthernet interfaces.
- Assigned gateway IP addresses for each VLAN.
- Enabled routing between VLAN 10 and VLAN 20.

---

## Verification

### Successful Tests

- VLAN 10 devices communicated with VLAN 20 devices.
- Devices successfully reached their configured default gateways.
- End-to-end connectivity was verified using ICMP (ping).

---

## Observation

The first ping between devices on different VLANs timed out, while subsequent pings succeeded.

Using `arp -a`, I observed that the ARP cache was populated after the initial request. The first ICMP packet was delayed while the sending host resolved the MAC address of its default gateway. Once the ARP entry was cached, the remaining packets were forwarded successfully.

---

## Commands Used

- `ping`
- `arp -a`

---

## What I Learned

- VLANs isolate Layer 2 broadcast domains.
- Routers enable communication between different VLANs.
- Hosts send traffic destined for another subnet to their default gateway.
- ARP resolves IP addresses to MAC addresses before Ethernet frames can be transmitted.
- Initial communication may experience a delay while ARP entries are learned and cached.

---

## Technologies

- Cisco Packet Tracer
- Cisco 2911 Router
- Cisco Catalyst 2960 Switch
- IPv4
- VLANs
- Inter-VLAN Routing
- ARP
- ICMP
