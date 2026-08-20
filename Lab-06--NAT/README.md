# NAT & Port Forwarding Lab

## Overview

This lab demonstrates Network Address Translation (NAT) and port forwarding using Cisco Packet Tracer.

The network is divided into an outside network and a private internal network. A Cisco router sits between the two networks and performs NAT.

The lab demonstrates three important behaviors:

1. Private hosts can initiate communication with the outside network.
2. Outside hosts cannot directly initiate connections to private hosts.
3. A specific service on a private host can be intentionally exposed to the outside network through the router using port forwarding.

---

## Objectives

- Configure a private IPv4 network
- Configure an outside network
- Configure NAT on a Cisco router
- Allow private hosts to communicate with the outside network
- Test inbound connectivity from the outside network
- Configure port forwarding to an internal web server
- Verify NAT behavior using ICMP and HTTP
- Understand the difference between private host accessibility and published services

---

## Network Topology

### Outside Network

```text
20.20.20.0/24
```

Devices on this network represent hosts outside of the private network.

| Device | IP Address |
|---|---|
| PC0 | 20.20.20.2 |
| PC1 | 20.20.20.1 |

### Private Network

```text
10.10.10.0/24
```

Devices on this network represent internal hosts behind the NAT router.

| Device | IP Address |
|---|---|
| PC2 | 10.10.10.1 |
| Web Server | 10.10.10.2 |

The router separates the two networks and performs NAT between them.

---

## Network Diagram

The topology consists of:

```text
                  OUTSIDE NETWORK
                  20.20.20.0/24
                         |
                         |
                  +-------------+
                  |   NAT       |
                  |   Router    |
                  +-------------+
                         |
                         |
                  PRIVATE NETWORK
                  10.10.10.0/24
                     /       \
                    /         \
                  PC2       Web Server
             10.10.10.1    10.10.10.2
```

The router provides the boundary between the outside and private networks.

---

# NAT Behavior

NAT allows hosts using private IP addresses to communicate with an outside network.

For example:

```text
10.10.10.1
    |
    | Outbound traffic
    v
  Router
    |
    v
20.20.20.X
```

A private host was able to successfully communicate with a host on the outside network.

This demonstrates how NAT allows internal hosts to initiate outbound connections without requiring their private IP addresses to be directly routable on the outside network.

---

# Outside-to-Private Connectivity

An outside host was also tested against a private host.

For example:

```text
20.20.20.X → 10.10.10.1
```

The outside host was unable to directly initiate communication with the private host.

This demonstrates that the private host's address is not directly reachable from the outside network through the NAT configuration.

---

# Port Forwarding to an Internal Web Server

The private network contains a web server:

```text
10.10.10.2
```

The web server is located behind the NAT router and uses a private IP address.

Rather than allowing outside hosts to directly access `10.10.10.2`, port forwarding was configured on the router.

An outside PC can enter the router's outside-facing IP address into its web browser:

```text
http://<router-outside-ip>
```

The router then forwards the HTTP request to:

```text
10.10.10.2
```

### Traffic Flow

```text
Outside PC
     |
     | HTTP request
     | Router's outside IP
     v
+------------+
|   Router   |
|    NAT     |
+------------+
     |
     | Port forwarding
     v
10.10.10.2
Web Server
```

The outside host therefore communicates with the router rather than directly addressing the private web server.

This demonstrates how a specific internal service can be published externally without making the entire private network directly accessible.

---

# Testing

## Test 1 — Private Host → Outside Host

A private host successfully pinged a host on the outside network.

**Result: PASS**

```text
10.10.10.1 → 20.20.20.X
```

The private host was able to initiate communication with the outside network.

---

## Test 2 — Outside Host → Private Host

An outside host attempted to directly ping a private host.

**Result: BLOCKED**

```text
20.20.20.X → 10.10.10.1
```

The outside host could not directly initiate communication with the private host.

---

## Test 3 — Outside Host → Internal Web Server

An outside PC entered the router's outside-facing IP address into its web browser.

The router forwarded the HTTP request to the internal web server.

**Result: PASS**

```text
Outside PC
     ↓
Router Outside IP
     ↓
NAT / Port Forwarding
     ↓
10.10.10.2
Web Server
```

The Cisco Packet Tracer web page hosted by the internal server was successfully displayed on the outside PC.

---

# Key Concepts

### Private IP Addresses

The internal network uses private IPv4 addressing:

```text
10.10.10.0/24
```

These addresses are intended for internal networks and are not directly routable across the public Internet.

### NAT

Network Address Translation allows private hosts to communicate with outside networks by translating network addresses at the router.

### Port Forwarding

Port forwarding allows an incoming connection to the router to be forwarded to a specific host and service inside the private network.

In this lab:

```text
Router Outside IP
        ↓
      HTTP
        ↓
10.10.10.2 Web Server
```

### Controlled External Access

The lab demonstrates the difference between:

**Direct access to a private host:**

```text
Outside → 10.10.10.1
```

Blocked.

And:

**Access to an intentionally published service:**

```text
Outside → Router → 10.10.10.2:HTTP
```

Allowed.

This allows a specific service to be exposed without making the entire private network directly accessible.

---

# Verification

The following Cisco IOS commands can be used to verify the configuration and operation of NAT.

### View NAT Translations

```cisco
show ip nat translations
```

Displays active NAT translation entries.

### View NAT Statistics

```cisco
show ip nat statistics
```

Displays NAT statistics and interface information.

### View Interface Status

```cisco
show ip interface brief
```

Displays interface IP addresses and operational status.

### View Running Configuration

```cisco
show running-config
```

Displays the current router configuration.

### Test Connectivity

```text
ping <destination-ip>
```

HTTP connectivity was tested using the web browser included in Cisco Packet Tracer.

---

# What I Learned

- How NAT allows private hosts to communicate with outside networks
- How private IP addresses are separated from outside networks
- Why outside hosts cannot directly reach private hosts through the NAT configuration
- How port forwarding can expose a specific internal service
- How an outside client can access an internal web server through the router's outside IP
- The difference between accessing a private host directly and accessing a published service
- How to test NAT behavior using ICMP
- How to test an exposed service using HTTP
- How to verify NAT translations and statistics using Cisco IOS commands
- How NAT can provide network address translation while allowing controlled access to internal services

---

## Technologies

- Cisco Packet Tracer
- Cisco IOS
- Cisco Routers
- Network Address Translation (NAT)
- Port Forwarding
- IPv4
- Private IP Addressing
- ICMP
- HTTP
