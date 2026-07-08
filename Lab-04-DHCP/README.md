# Lab 04 - DHCP with DHCP Relay (IP Helper Address)

## Objective

Deploy a centralized DHCP server that automatically assigns IP addresses to multiple subnets using DHCP Relay (IP Helper Address) configured on a Cisco router.

---

## Skills Learned

- DHCP
- DHCP Relay (IP Helper Address)
- Router Configuration
- Default Gateways
- Automatic IP Address Assignment
- Network Services
- Connectivity Testing

---

## Network Topology

```
                     Cisco 2911 Router
                    -------------------
                  G0/0             G0/1
                    |                 |
              Switch 0          Switch 1
                 |                  |
        DHCP Server            Client Devices
         PC Clients           Laptop Clients
```

---

## Network Information

### Subnet A

Network: `192.168.10.0/24`

Gateway: `192.168.10.1`

Connected Devices:
- DHCP Server
- PCs

---

### Subnet B

Network: `192.168.20.0/24`

Gateway: `192.168.20.1`

Connected Devices:
- PCs
- Laptops

---

## Configuration Summary

### Router

Configured:

- GigabitEthernet interfaces
- Default gateway addresses
- DHCP Relay using `ip helper-address`

---

### DHCP Server

Configured:

- DHCP Service
- Address Pools
- Subnet Masks
- Default Gateway
- DNS Information

---

### Clients

Configured all devices to obtain their network settings automatically using DHCP.

---

## Verification

Verified that:

- Every client successfully obtained an IP address automatically.
- Clients received the correct subnet mask.
- Clients received the correct default gateway.
- Clients from different subnets could communicate successfully.
- End-to-end connectivity was confirmed using ICMP (ping).

---

## Troubleshooting

Issue:

Clients on the remote subnet were not receiving DHCP addresses.

Investigation:

Verified router interface configuration and DHCP server settings.

Resolution:

Configured `ip helper-address` on the router interface. DHCP requests were then forwarded to the centralized DHCP server, allowing remote clients to receive IP configurations successfully.

---

## What I Learned

- DHCP automatically assigns IP addresses to clients.
- DHCP Discover messages are broadcasts and do not cross routers by default.
- Cisco's `ip helper-address` forwards DHCP broadcasts as unicast packets to a DHCP server on another network.
- Routers enable centralized DHCP services across multiple subnets.
- Automatic IP assignment greatly simplifies network administration.

---

## Commands and Technologies

- DHCP
- DHCP Relay
- `ip helper-address`
- IPv4
- Cisco Packet Tracer
- Cisco 2911 Router
- Cisco Catalyst 2960 Switch
- ICMP
- Ping
