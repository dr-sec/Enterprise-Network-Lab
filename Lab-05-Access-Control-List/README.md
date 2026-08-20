# Access Control List (ACL) Lab

## Overview

This lab demonstrates the use of Cisco Standard Access Control Lists (ACLs) to control traffic between different networks.

The lab was built in Cisco Packet Tracer and uses two routers connected through OSPF. Two ACL configurations were tested:

1. Blocking traffic from a single host
2. Blocking traffic from an entire subnet

The purpose of the lab was to understand how ACLs filter traffic and how the placement and direction of an ACL affects network communication.

---

## Network Topology

### Networks

| Network | Purpose |
|---|---|
| 192.168.1.0/24 | Network behind R1 |
| 192.168.12.0/24 | Router-to-router network |
| 192.168.2.0/24 | Network behind R2 |

### Router Interfaces

#### R1

| Interface | IP Address |
|---|---|
| Fa0/0 | 192.168.1.1/24 |
| Fa0/1 | 192.168.12.1/24 |

#### R2

| Interface | IP Address |
|---|---|
| Fa0/0 | 192.168.12.2/24 |
| Fa0/1 | 192.168.2.1/24 |

---

# Routing Configuration

OSPF was configured to allow R1 and R2 to learn routes to each other's networks.

## R1

```cisco
enable
configure terminal
hostname R1

interface f0/0
ip address 192.168.1.1 255.255.255.0
no shutdown

interface f0/1
ip address 192.168.12.1 255.255.255.0
no shutdown

router ospf 1

interface f0/0
ip ospf 1 area 0

interface f0/1
ip ospf 1 area 0****



## R2

enable
configure terminal
hostname R2

interface f0/0
ip address 192.168.12.2 255.255.255.0
no shutdown

interface f0/1
ip address 192.168.2.1 255.255.255.0
no shutdown

router ospf 1

interface f0/0
ip ospf 1 area 0

interface f0/1
ip ospf 1 area 0
