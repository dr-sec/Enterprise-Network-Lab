# OSPF Dynamic Routing Lab

## Overview

This lab demonstrates the configuration of **Open Shortest Path First (OSPF)** dynamic routing using Cisco routers in Cisco Packet Tracer.

The network consists of four routers connected in a redundant topology, with two separate LANs attached to the network. OSPF Area 0 was configured across all router interfaces to allow the routers to dynamically learn routes to remote networks.

The lab was also used to troubleshoot a connectivity issue caused by a physical interface/link failure between R2 and R3.

### Objectives

* Configure IPv4 addresses on router interfaces
* Configure OSPF on multiple routers
* Establish OSPF neighbor adjacencies
* Allow routers to dynamically learn remote networks
* Verify routing tables
* Test end-to-end connectivity between different LANs
* Troubleshoot an interface that was showing `down/down`

---

## Network Topology

The topology consists of four routers connected in a redundant network:

```text
                    R2
              /             \
             /               \
           R1                 R3
            \               /
             \             /
                    R4
```

R1 and R3 each provide connectivity to a separate LAN.

### Networks

| Network           | Purpose |
| ----------------- | ------- |
| `192.168.1.0/24`  | R1 LAN  |
| `192.168.12.0/24` | R1 ↔ R2 |
| `192.168.23.0/24` | R2 ↔ R3 |
| `192.168.34.0/24` | R3 ↔ R4 |
| `192.168.14.0/24` | R4 ↔ R1 |
| `192.168.2.0/24`  | R3 LAN  |

---

# IP Addressing

## R1

| Interface | IP Address        | Purpose     |
| --------- | ----------------- | ----------- |
| Fa0/0     | `192.168.12.1/24` | R1 ↔ R2     |
| Fa0/1     | `192.168.14.1/24` | R1 ↔ R4     |
| Fa1/0     | `192.168.1.1/24`  | LAN gateway |

## R2

| Interface | IP Address        | Purpose |
| --------- | ----------------- | ------- |
| Fa0/0     | `192.168.12.2/24` | R2 ↔ R1 |
| Fa0/1     | `192.168.23.1/24` | R2 ↔ R3 |

## R3

| Interface | IP Address        | Purpose     |
| --------- | ----------------- | ----------- |
| Fa0/0     | `192.168.23.2/24` | R3 ↔ R2     |
| Fa0/1     | `192.168.34.1/24` | R3 ↔ R4     |
| Fa1/0     | `192.168.2.1/24`  | LAN gateway |

## R4

| Interface | IP Address        | Purpose |
| --------- | ----------------- | ------- |
| Fa0/0     | `192.168.34.2/24` | R4 ↔ R3 |
| Fa0/1     | `192.168.14.2/24` | R4 ↔ R1 |

---

# OSPF Configuration

OSPF Process ID `1` was configured on all four routers.

All participating interfaces were placed into **Area 0**.

## R1

```cisco
enable
configure terminal
hostname R1

interface f0/0
ip address 192.168.12.1 255.255.255.0
no shutdown

interface f0/1
ip address 192.168.14.1 255.255.255.0
no shutdown

interface f1/0
ip address 192.168.1.1 255.255.255.0
no shutdown

router ospf 1

interface f0/0
ip ospf 1 area 0

interface f0/1
ip ospf 1 area 0

interface f1/0
ip ospf 1 area 0
```

## R2

```cisco
enable
configure terminal
hostname R2

interface f0/0
ip address 192.168.12.2 255.255.255.0
no shutdown

interface f0/1
ip address 192.168.23.1 255.255.255.0
no shutdown

router ospf 1

interface f0/0
ip ospf 1 area 0

interface f0/1
ip ospf 1 area 0
```

## R3

```cisco
enable
configure terminal
hostname R3

interface f0/0
ip address 192.168.23.2 255.255.255.0
no shutdown

interface f0/1
ip address 192.168.34.1 255.255.255.0
no shutdown

interface f1/0
ip address 192.168.2.1 255.255.255.0
no shutdown

router ospf 1

interface f0/0
ip ospf 1 area 0

interface f0/1
ip ospf 1 area 0

interface f1/0
ip ospf 1 area 0
```

## R4

```cisco
enable
configure terminal
hostname R4

interface f0/0
ip address 192.168.34.2 255.255.255.0
no shutdown

interface f0/1
ip address 192.168.14.2 255.255.255.0
no shutdown

router ospf 1

interface f0/0
ip ospf 1 area 0

interface f0/1
ip ospf 1 area 0
```

---

# Verification

Several commands were used to verify the OSPF configuration and troubleshoot connectivity.

### Check interface status

```cisco
show ip interface brief
```

This was particularly useful during troubleshooting.

### Check OSPF neighbors

```cisco
show ip ospf neighbor
```

This verifies whether routers have successfully established OSPF adjacencies.

### Check the routing table

```cisco
show ip route
```

OSPF-learned routes should appear with an `O` designation.

Example:

```text
O    192.168.2.0/24
```

### Check OSPF interfaces

```cisco
show ip ospf interface brief
```

This verifies which interfaces are participating in OSPF and which area they belong to.

---

# Troubleshooting

During the lab, the R2–R3 connection initially showed a **red link indicator**.

The interface status revealed:

```text
R2 Fa0/1    192.168.23.1    up/up
R3 Fa0/0    192.168.23.2    down/down
```

R2's interface was operational, but R3's interface was not detecting a working link.

The IP addressing and OSPF configuration were reviewed first. After confirming the configurations were correct, the physical connection between R2 and R3 was deleted and reconnected in Packet Tracer.

After reconnecting the cable, the interface transitioned to `up/up` and the OSPF adjacency was able to form.
That's **much stronger evidence for a NOC/IT networking application.**
