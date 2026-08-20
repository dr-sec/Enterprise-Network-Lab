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

## Routing Configuration

OSPF was configured to allow R1 and R2 to learn routes to each other's networks.

### R1

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
ip ospf 1 area 0
```

### R2

```cisco
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
```

---

## ACL Test 1 — Block a Single Host

The first test blocked traffic from only one device:

```text
192.168.1.2
```

The rest of the `192.168.1.0/24` network remained allowed.

### Configuration

```cisco
access-list 10 deny host 192.168.1.2
access-list 10 permit any

interface f0/0
ip access-group 10 in
```

### Result

- `192.168.1.2` → Blocked
- Other hosts on `192.168.1.0/24` → Allowed

The ACL was applied inbound on R1's `Fa0/0` interface, meaning traffic entering R1 from the `192.168.1.0/24` network was evaluated against ACL 10.

---

## ACL Test 2 — Block an Entire Subnet

The second test blocked traffic originating from the entire:

```text
192.168.1.0/24
```

subnet.

### Configuration

```cisco
access-list 10 deny 192.168.1.0 0.0.0.255
access-list 10 permit any

interface f0/0
ip access-group 10 in
```

### Result

- Entire `192.168.1.0/24` subnet → Blocked
- Other networks → Allowed

The wildcard mask `0.0.0.255` allows the ACL to match any host within the `192.168.1.0/24` network.

---

## Standard ACL Logic

Standard ACLs filter traffic based primarily on the **source IP address**.

### Block a Single Host

```cisco
access-list 10 deny host 192.168.1.2
```

This denies traffic originating from the specific host `192.168.1.2`.

### Block an Entire Subnet

```cisco
access-list 10 deny 192.168.1.0 0.0.0.255
```

This denies traffic originating from the entire `192.168.1.0/24` network.

---

## Wildcard Mask

The subnet ACL uses the wildcard mask:

```text
0.0.0.255
```

Unlike a subnet mask, a wildcard mask uses:

- `0` = Must match
- `255` = Can be anything

Therefore:

```text
192.168.1.0 0.0.0.255
```

matches:

```text
192.168.1.0
192.168.1.1
192.168.1.2
...
192.168.1.255
```

This allows the ACL to match the entire `/24` network.

---

## ACL Processing

ACL entries are processed from **top to bottom**.

For example:

```cisco
access-list 10 deny host 192.168.1.2
access-list 10 permit any
```

Traffic from `192.168.1.2` matches the first rule and is denied.

Traffic from other hosts does not match the first rule, so it continues to the second rule and is permitted.

ACLs also have an **implicit deny** at the end of every ACL.

This means that if traffic does not match any configured rule, it is ultimately denied.

---

## Verification

### Verify ACLs

```cisco
show access-lists
```

### Verify ACL Placement

```cisco
show ip interface f0/0
```

### Verify OSPF Neighbors

```cisco
show ip ospf neighbor
```

### Verify Routing Table

```cisco
show ip route
```

### Test Connectivity

```text
ping <destination IP>
```

---

## What I Learned

- How Standard ACLs filter traffic based on source IP addresses
- How to block an individual host
- How to block an entire subnet using a wildcard mask
- How ACLs are applied to router interfaces
- How inbound ACLs affect traffic entering an interface
- How ACL entries are processed sequentially
- The purpose of the implicit deny
- How OSPF provides routing between networks before ACL filtering is applied
- How to verify ACLs and routing using Cisco IOS commands
- How to troubleshoot OSPF when an interface is not participating in the routing process

---

## Technologies

- Cisco Packet Tracer
- Cisco IOS
- Cisco Routers
- OSPF
- IPv4
- Standard ACLs
- Wildcard Masks
- ICMP
