# IPv6: Header, NDP, SLAAC and Static Routing

CCNA study notes (Part 2): the IPv6 header, solicited-node multicast, NDP, SLAAC, DAD and the start of IPv6 static routing.

## Table of Contents

1. [Overview](#1-overview)
2. [The IPv6 Header](#2-the-ipv6-header)
3. [Solicited-Node Multicast Address](#3-solicited-node-multicast-address)
4. [NDP (Neighbor Discovery Protocol)](#4-ndp-neighbor-discovery-protocol)
5. [IPv6 Static Routing](#5-ipv6-static-routing)
6. [Quick Summary](#6-quick-summary)

---

## 1. Overview

In this part we apply the static routing concepts you already know from IPv4 to IPv6. Topics to cover:

- The IPv6 header
- Solicited-node multicast address
- NDP (Neighbor Discovery Protocol), which replaces ARP in IPv6
- SLAAC and DAD
- IPv6 static routing. **This is the most important section; it is right in the exam topic list.**

## 2. The IPv6 Header

When one computer sends information to another, it needs to add some information so the network knows where the packet came from, where it needs to go, and how it should be handled. This information, placed at the beginning of the packet, is the **IPv6 header**.

Think of sending a physical package: before sending it you write the sender, the receiver and handling instructions on it. The IPv6 header does the same for a network packet.

### The 8 fields

| #   | Field               | Size     | What it does |
| --- | ------------------- | -------- | ------------ |
| 1   | Version             | 4 bits   | Tells the device which version of IP is used. For IPv6 the value is **6**. |
| 2   | Traffic Class       | 8 bits   | Tells the network how to treat the packet (QoS). Voice and video are sensitive to delay, so they may need special treatment compared with a normal file transfer. |
| 3   | Flow Label          | 20 bits  | Identifies packets that belong to the same flow of communication, for example all the packets of one video call. |
| 4   | Payload Length      | 16 bits  | The size, in bytes, of what comes after the IPv6 header (the payload, i.e. the actual data). |
| 5   | Next Header         | 8 bits   | Tells what comes immediately after the IPv6 header: a TCP header, a UDP header, or an IPv6 extension header. |
| 6   | Hop Limit           | 8 bits   | How many routers (hops) the packet may pass through. Each router decreases it by 1. At 0 the packet is discarded, which stops packets looping forever if there is a routing problem. |
| 7   | Source Address      | 128 bits | The IPv6 address of the device that sent the packet (the "From" address on the envelope). |
| 8   | Destination Address | 128 bits | The IPv6 address of the device that should receive the packet (the "To" address on the envelope). |

The IPv6 header is a fixed **40 bytes**: 4 + 8 + 20 + 16 + 8 + 8 = 64 bits (8 bytes) of fields, plus 128 + 128 = 256 bits (32 bytes) of addresses.

### Put it all together

Computer A wants to send a message to Computer B across two routers:

```
Computer A ----- Router ----- Router ----- Computer B
```

The packet carries a header that says:

```
Version          -> I am using IPv6.
Traffic Class    -> This is how I should be treated. (QoS)
Flow Label       -> I belong to this communication flow.
Payload Length   -> This is the size of what comes after my header.
Next Header      -> This is what comes next.
Hop Limit        -> I can pass through this many routers.
Source Address   -> I came from Computer A.
Destination      -> I am going to Computer B.
```

> **Summary:** The IPv6 header is the packet's instruction and identification information. It says what type of packet it is, how it should be treated, what comes after the header, how far it can travel, where it came from and where it is going. The actual message comes after the header.

## 3. Solicited-Node Multicast Address

- An IPv6 solicited-node multicast address is **calculated from the unicast address**.
- It is the multicast prefix `FF02::1:FF00:0/104` (written in full: `FF02:0000:0000:0000:0000:0001:FF00:0000`) plus the **last 6 hex digits** of the unicast address.

**Example**

| Step                     | Result                                          |
| ------------------------ | ----------------------------------------------- |
| Unicast address          | `2001:0db8:0000:0001:0f2a:4fff:fea3:00b1`       |
| Last 6 hex digits        | `a3:00b1` becomes `A300B1`                      |
| Solicited-node multicast | `FF02::1:FFA3:00B1` (shortened: `ff02::1:ffa3:b1`) |

## 4. NDP (Neighbor Discovery Protocol)

- NDP is a protocol used with IPv6. It is an essential part of IPv6 routing.
- It has various functions. One of them replaces **ARP**, which is no longer used in IPv6.
- The ARP-like function of NDP uses **ICMPv6** and the **solicited-node multicast address** to learn the MAC address of other hosts.

| Message                      | ICMPv6 type | Meaning                                                         |
| ---------------------------- | ----------- | --------------------------------------------------------------- |
| NS (Neighbor Solicitation)   | 135         | "Who has this IPv6 address?"                                    |
| NA (Neighbor Advertisement)  | 136         | "I have it; this is me."                                        |
| RS (Router Solicitation)     | 133         | "Is there an IPv6 router here?"                                 |
| RA (Router Advertisement)    | 134         | "Yes, I am here, and here is information about the network."    |

### 4.1 Address resolution: NS and NA

- **Neighbor Solicitation (NS)** is used when an IPv6 device wants information about another device on the local network. For example, PC1 knows PC2's IPv6 address but not its MAC address. PC1 sends an NS asking "Who has this IPv6 address?" (ICMPv6 type 135).
- **Neighbor Advertisement (NA)** is the response. When PC2 realizes the address belongs to it, it sends an NA back to PC1: "Yes, that address belongs to me, and here is my information."
- NS = "Who has this IPv6 address?" NA = "I have it; this is me."

### 4.2 Router discovery: RS and RA

Another NDP function lets a device automatically discover routers on its local network. This is useful because a computer may connect to a network without knowing which router to use to reach other networks.

- **Router Solicitation (RS):** sent by a host (such as a PC) to find out whether an IPv6 router is on the local network. "Is there an IPv6 router here? If so, tell me about yourself and the network."
- **Router Advertisement (RA):** sent by an IPv6 router to give information about the network: "I am the router you can use to reach other networks", the IPv6 network prefix and other configuration information. The host can then communicate outside its local network.
- RS = the host asks "Is there a router here?" RA = the router answers "Yes, I am here, and here is information about the network."

### 4.3 SLAAC

SLAAC (Stateless Address Autoconfiguration) lets a PC configure its own IPv6 address automatically. The PC receives an RA from the router.

| Step | What happens |
| ---- | ------------ |
| 1    | The router's RA tells the PC the IPv6 network prefix, for example `2001:db8:1234:1::/64`. The first part of the address belongs to the network. |
| 2    | The PC still needs the remaining 64 bits, the **Interface ID**. It creates this itself, using a randomly generated interface ID or another method such as Modified EUI-64. |
| 3    | The PC combines the prefix from the router with its own interface ID to build the complete address. |

Example: the router provides `2001:db8:1234:1::/64` and the PC creates the interface ID `abcd:1234:5678:90ab`. The final address is:

```
2001:db8:1234:1:abcd:1234:5678:90ab
```

> **In simple terms:** the router gives the PC the first half of the address, and the PC creates the second half itself. A DHCPv6 server does not need to assign the complete address.

### 4.4 DAD (Duplicate Address Detection)

- DAD lets a device check whether the IPv6 address it wants to use is already used by another device on the same network.
- Example: a PC uses SLAAC to create `2001:db8:1::10`. It does not start using it immediately. First it performs DAD, asking "Is anyone already using this IPv6 address?"
- The PC sends a Neighbor Solicitation (NS) message to check. If another device is already using the address, it can respond, and the PC knows the address is duplicated and must not be used.
- If no device responds, the PC assumes the address is unique and can start using it.
- DAD is IPv6's way of making sure two devices on the same network do not use the same IPv6 address.

## 5. IPv6 Static Routing

- IPv6 routing works the same way as IPv4 routing: a packet arrives on one interface, the router looks up the destination IP address in its routing table, and forwards the packet according to the **most specific match**.
- Although the process is the same, IPv4 and IPv6 routing are **separate processes** on the router, and the two routing tables are separate as well.

|               | IPv4 routing        | IPv6 routing           |
| ------------- | ------------------- | ---------------------- |
| Default state | Enabled by default  | **Disabled** by default |
| How to enable | Nothing to do       | `ipv6 unicast-routing` |

- If IPv6 routing is disabled, the router can still send and receive IPv6 traffic, but it will **not route** it (it will not forward it between networks).

Enable IPv6 routing in global configuration mode:

```
Router(config)# ipv6 unicast-routing
```

> **Always make sure to use:** `ipv6 unicast-routing`

## 6. Quick Summary

- IPv6 header = 8 fields, fixed 40 bytes: Version, Traffic Class, Flow Label, Payload Length, Next Header, Hop Limit, Source, Destination.
- Solicited-node multicast = `FF02::1:FF00:0/104` + last 6 hex digits of the unicast address.
- NDP replaces ARP. NS (135) / NA (136) resolve addresses. RS (133) / RA (134) discover routers.
- SLAAC: the router's RA gives the prefix, the PC builds its own 64-bit interface ID.
- DAD: the PC checks, using an NS, that nobody else has its address before using it.
- IPv6 routing is disabled by default. Enable it with `ipv6 unicast-routing`.
