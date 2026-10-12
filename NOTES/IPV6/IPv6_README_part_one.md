# IPv6

CCNA study notes on IPv6: why it exists, address format, prefixes, EUI-64 and all the address types.

## Table of Contents

1. [Why IPv6?](#1-why-ipv6)
2. [IPv6 Introduction](#2-ipv6-introduction)
3. [Shortening IPv6 Addresses](#3-shortening-ipv6-addresses)
4. [Finding the Prefix (Global Unicast Address)](#4-finding-the-prefix-global-unicast-address)
5. [EUI-64](#5-eui-64)
6. [IPv6 Address Types](#6-ipv6-address-types)
7. [Quick Summary](#7-quick-summary)

---

## 1. Why IPv6?

- The main reason is that there are simply not enough IPv4 addresses. IPv4 addresses are only 32 bits long.
- When IPv4 was designed about 30 years ago, its creators had no idea the internet would grow this large.
- VLSM, private IPv4 addresses and NAT have been used to conserve IPv4 address space. These are only short-term solutions.
- **The long-term solution is IPv6.**

**Who controls address assignment?**

- IPv4 address assignment is controlled by **IANA** (Internet Assigned Numbers Authority).
- IANA distributes IPv4 address space to the **RIRs** (Regional Internet Registries), which then assign it to companies that need it.

## 2. IPv6 Introduction

- An IPv6 address is **128 bits** long, four times the length of an IPv4 address (32 bits).
- Four times the bits does **not** mean four times the addresses. Every extra bit **doubles** the number of possible addresses.
- 32 bits gives about 4 billion addresses, 33 bits about 8 billion, 34 bits about 16 billion, and so on.
- IPv6 total = 2^128 addresses (about 3.4 × 10^38).
- Addresses are written in **hexadecimal**, in **8 groups (quartets)** of 4 hex digits (16 bits each).

Example:

```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

- IPv6 uses **/ prefix-length notation**, even when configuring the address on a Cisco IOS device. There are no dotted-decimal subnet masks.
- For example, `/64` means the first half of the address is the network portion and the second half is the host portion.

## 3. Shortening IPv6 Addresses

Written in full, IPv6 addresses are long and hard to read. There are two methods to shorten them, and they can be combined.

### Method 1: Remove leading zeros

Leading zeros are the zeros at the beginning of any quartet. They can simply be removed.

| Full                                     | Shortened                      |
| ---------------------------------------- | ------------------------------ |
| `2001:0DB8:000A:001B:20A1:0080:34DB:0020` | `2001:DB8:A:1B:20A1:80:34DB:20` |

### Method 2: Replace consecutive all-zero quartets with `::`

| Full                                     | Shortened           |
| ---------------------------------------- | ------------------- |
| `2001:0DB8:0000:0000:0000:0000:0080:34BD` | `2001:DB8::80:34BD` |

### Limitation: use `::` only once per address

Consecutive quartets of zeros can be abbreviated with `::` only **once** in an IPv6 address. Take this address:

```
2001:0000:0000:0000:20A1:0000:0000:34DB
```

It is tempting to write `2001::20A1::34DB`, but this is **invalid**. A device reading it cannot tell how many zero quartets each `::` stands for, so the address becomes ambiguous.

Correct option (shorten the longest run with `::` and leave the other zeros as single `0`s):

```
2001::20A1:0:0:34DB
```

## 4. Finding the Prefix (Global Unicast Address)

### How an enterprise gets its addresses

- An enterprise requests IPv6 addresses from its ISP and typically receives a **/48** block (the global routing prefix).
- Standard IPv6 subnets use a **/64** prefix length.
- From /48 to /64 leaves **16 bits** for subnetting, so the enterprise can create 2^16 = **65,536 subnets** (for example one per department, building or segment).
- Each /64 subnet has **64 bits** for hosts, which is 2^64 possible hosts per subnet. You will never need that many, but /64 is the convention.
- The host portion is fixed at 64 bits to support automatic address configuration (such as EUI-64) and to keep networks consistent.

### Worked example

```
2001:0DB8:8B00:0001:0000:0000:0000:0001/64
```

| Part                            | Value                 | Meaning                                        |
| ------------------------------- | --------------------- | ---------------------------------------------- |
| Global routing prefix (48 bits) | `2001:0DB8:8B00`      | The /48 block assigned by the ISP              |
| Subnet ID (16 bits)             | `0001`                | Free bits the enterprise uses to make subnets  |
| Interface ID (64 bits)          | `0000:0000:0000:0001` | The host portion                               |

> **Remember:** Global routing prefix + Subnet ID = the network portion (the IPv6 network prefix). The last 64 bits = the host portion.

## 5. EUI-64

- EUI stands for **Extended Unique Identifier**.
- EUI-64 is a method of converting a **48-bit MAC address** into a **64-bit interface identifier**.
- This interface identifier becomes the host portion of a /64 IPv6 address (64 network bits + 64 host bits = 128 bits).
- **EUI-64 is not a type of IPv6 address.** It is a method of automatically generating the host part of an address from a specified prefix and a MAC address.
- When you configure this on a router, the conversion is done automatically.

### Conversion steps

1. Split the MAC address in half (6 hex digits | 6 hex digits).
2. Insert `FFFE` in the middle, between the 6th and 7th hex digits.
3. Invert the **7th bit** of the first byte (the U/L bit). If it is 0, make it 1. If it is 1, make it 0.

### Example

| Step                                            | Result                |
| ----------------------------------------------- | --------------------- |
| MAC address                                     | `1234.5678.90AB`      |
| 1. Split in half                                | `123456 \| 7890AB`    |
| 2. Insert FFFE                                  | `123456FFFE7890AB`    |
| 3. Flip 7th bit (`12` = 00010010 becomes 00010000 = `10`) | `1034:56FF:FE78:90AB` |

With the prefix `2001:DB8:8B00:1::/64`, the full address becomes `2001:DB8:8B00:1:1034:56FF:FE78:90AB`.

Cisco IOS command (the router does the conversion for you):

```
Router(config-if)# ipv6 address 2001:db8:8b00:1::/64 eui-64
```

## 6. IPv6 Address Types

IPv6 has three main kinds of address. There is no broadcast in IPv6; multicast takes over that role.

| Type           | Range / prefix                  | Short description                              |
| -------------- | ------------------------------- | ---------------------------------------------- |
| Global unicast | `2000::/3` (originally)         | Public, internet-routable, one device          |
| Unique local   | `FC00::/7` (in practice `FD00::/8`) | Private, not routable on the internet      |
| Link-local     | `FE80::/10`                     | Same link only, auto-generated                 |
| Multicast      | `FF00::/8`                      | One sender, many receivers (a group)           |
| Anycast        | Taken from unicast space        | One address on many devices, nearest one answers |

### 6.1 Global Unicast Address

- A **public** address that can be used over the internet.
- An ISP can give an organization a global unicast prefix, commonly a /48. The organization divides it into /64 networks and assigns addresses to its devices.
- You must register to use them. Because they are public, they are expected to be **globally unique**, so organizations cannot invent them; they receive properly allocated address space.
- If two companies used the same global unicast range, there would be a problem, like two homes with exactly the same street address.
- The range originally defined for global unicast was the `2000::/3` block (`2000::` to `3FFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF`).
- This was later updated to include many more addresses. Now, all addresses that are not reserved for another purpose are global unicast addresses.

### 6.2 Unique Local Address

- A **private** address that cannot be used over the internet. It is like a private IPv4 address.
- You do not need to register to use them. They can be used freely inside internal networks and do not need to be globally unique.
- They cannot be routed over the internet. Your ISP will simply drop packets destined for a unique local address.
- The block reserved for them is `FC00::` to `FDFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF` (`FC00::/7`).
- A later update requires the 8th bit to be set to 1, so in practice **all unique local addresses begin with `FD`**.

### 6.3 Link-Local Address

- Automatically generated on IPv6-enabled interfaces. You can recognise them by `FE80::/10`.
- Used by a device to communicate with other devices on the **same local network (link)**.
- Cannot be routed across the internet or between different networks. For example, a PC can use `FE80::1` to talk to its local router, but not to reach a device on another network.
- **Link-local = `FE80...` = communication on the same local link.**

Enabling IPv6 on an interface:

```
Router(config-if)# ipv6 enable
```

- `ipv6 enable` turns on IPv6 processing on the interface, and the router automatically generates a link-local address (usually starting with `FE80`).
- It does **not** configure a global unicast address.

**Uses of link-local addresses (communication within a subnet):**

- Routing protocol peering (OSPFv3 uses link-local addresses for neighbor adjacencies).
- Next-hop addresses for static routes.
- Neighbor Discovery Protocol (NDP, the IPv6 replacement for ARP) uses link-local addresses to function.

### 6.4 Multicast Address

- A multicast address is used to send **one packet to multiple devices at the same time**.
- Instead of sending the same packet separately to each device, the sender sends it to a multicast address, and every device that belongs to that multicast group receives it.
- IPv6 multicast addresses always start with **`FF`**, so the range is **`FF00::/8`**.
- IPv6 has no broadcast. Multicast is used instead, which means devices that are not interested in the traffic do not have to process it.
- **`FF...` = Multicast = one sender, multiple devices.**

**Well-known multicast addresses:**

| Address             | Group                        | Used for                                          |
| ------------------- | ---------------------------- | ------------------------------------------------- |
| `FF02::1`           | All IPv6 nodes on the link   | Reaching every IPv6 device on the local link      |
| `FF02::2`           | All IPv6 routers on the link | Reaching all routers (for example router solicitation) |
| `FF02::5`           | All OSPFv3 routers           | OSPFv3 hello and updates                          |
| `FF02::6`           | OSPFv3 DR and BDR            | OSPFv3 on multi-access networks                   |
| `FF02::9`           | All RIPng routers            | RIPng updates                                     |
| `FF02::A`           | All EIGRP routers            | EIGRP for IPv6                                    |
| `FF02::1:FFxx:xxxx` | Solicited-node multicast     | NDP address resolution (replaces ARP broadcast)   |

**Reading the second quartet:** in `FF02`, the `02` means link-local scope, so the packet stays on the local link and is not routed to other networks.

Common scope values: `1` = interface-local, `2` = link-local, `5` = site-local, `E` = global.

### 6.5 Anycast Address

- An anycast address is assigned to **more than one device** (usually on different routers or servers). It uses the same address on all of them.
- When a packet is sent to an anycast address, it is delivered to only **one** of those devices: the **nearest** one, as measured by the routing protocol.
- **Anycast = one address, many devices, nearest one answers.**
- Anycast addresses are taken from the unicast address space. They are not a separate range, so they look exactly like normal unicast addresses.
- Because of this, a device must be told explicitly to treat an address as anycast. On a Cisco router, you add the `anycast` keyword:

```
Router(config-if)# ipv6 address 2001:db8:1::1/128 anycast
```

- Typical uses: services that need to be reachable from many places with low delay, such as DNS servers and content delivery networks. If the nearest server fails, routing sends traffic to the next nearest one.

**Unicast vs multicast vs anycast:**

| Type      | Who receives the packet                      | Remember                   |
| --------- | -------------------------------------------- | -------------------------- |
| Unicast   | One specific device                          | One to one                 |
| Multicast | Every device in the group                    | One to many                |
| Anycast   | Only the nearest device that has the address | One to the nearest of many |

### 6.6 Special Addresses

| Address   | Name        | Meaning                                                  |
| --------- | ----------- | -------------------------------------------------------- |
| `::/128`  | Unspecified | A device has no address yet (like `0.0.0.0` in IPv4)     |
| `::1/128` | Loopback    | A device talking to itself (like `127.0.0.1` in IPv4)    |

## 7. Quick Summary

- IPv6 = 128 bits, 8 quartets of hex, written with a `/` prefix length.
- Shorten by removing leading zeros and by replacing **one** run of all-zero quartets with `::`, only once.
- An enterprise gets a /48 from the ISP, uses 16 subnet bits, and each subnet is a /64 (64 host bits).
- EUI-64 = split the MAC, insert `FFFE` in the middle, flip the 7th bit.
- Global unicast: public, from `2000::/3` (and more now). Unique local: private, starts with `FD`. Link-local: `FE80::/10`, same link only.
- Multicast: `FF00::/8`, one to many. Anycast: same address on many devices, nearest one answers.
