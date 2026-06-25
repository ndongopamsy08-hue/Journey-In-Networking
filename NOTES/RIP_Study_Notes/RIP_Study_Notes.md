# 🌐 RIP — Routing Information Protocol
### Complete Study Notes | Networking Fundamentals

> **Who this is for:** Anyone learning CCNA/networking fundamentals who wants to understand RIP from scratch — what it is, how it works internally, and how to configure it.

---

## 📚 Table of Contents

1. [What is RIP?](#1-what-is-rip)
2. [Distance Vector IGP — Deep Dive](#2-distance-vector-igp--deep-dive)
3. [How RIP Learns Routes Step by Step](#3-how-rip-learns-routes-step-by-step)
4. [Hop Count & Metric](#4-hop-count--metric)
5. [RIP Messages — Request & Response](#5-rip-messages--request--response)
6. [RIPv1 vs RIPv2](#6-ripv1-vs-ripv2)
7. [Broadcast vs Multicast](#7-broadcast-vs-multicast)
8. [Configuring RIPv2](#8-configuring-ripv2)
9. [The `network` Command — How It Really Works](#9-the-network-command--how-it-really-works)
10. [Auto-Summary — Why It Matters](#10-auto-summary--why-it-matters)
11. [Passive Interface](#11-passive-interface)
12. [RIP Timers](#12-rip-timers)
13. [RIP Limitations & When NOT to Use It](#13-rip-limitations--when-not-to-use-it)
14. [Quick Reference Cheat Sheet](#14-quick-reference-cheat-sheet)

---

## 1. What is RIP?

**RIP** stands for **Routing Information Protocol**.

It is one of the **oldest and simplest** routing protocols still in use today. RIP is categorized as a:

| Category | What it means |
|---|---|
| **Distance Vector** | Routers share their routing table with neighbors. No router sees the full network map. |
| **IGP** (Interior Gateway Protocol) | Used *inside* a single organization's network (a company, campus, etc.) |
| **Classful (v1) / Classless (v2)** | RIPv1 assumes default subnet masks; RIPv2 includes subnet mask information |

**In one sentence:**
> RIP is a distance vector IGP where routers gossip routes to their neighbors, and the best path is always chosen by counting hops — the router with the fewest jumps to the destination wins.

---

## 2. Distance Vector IGP — Deep Dive

Understanding "Distance Vector IGP" is the foundation of understanding RIP.

### 🔍 Breaking Down Each Word

#### "Routing by Rumor" (the core idea)
Routers using RIP **never see the full network map**. Instead, they only trust what their directly connected neighbors tell them. This is called **routing by rumor** because:

- Router A tells Router B: *"I can reach Network X in 1 hop."*
- Router B adds 1 hop to its own count and tells Router C: *"Through me, you can reach Network X in 2 hops."*
- Router C now knows about Network X — even though it never talked to Router A directly.

The knowledge travels like gossip — hop by hop, neighbor by neighbor.

---

#### 📏 "Distance" = Hop Count

In RIP, **distance** is measured in **hops**.

> **A hop** = one router your packet passes through.

```
Source → [Router A] → [Router B] → [Router C] → Destination
                  hop 1       hop 2       hop 3
```

That path above = **3 hops**.

If there's another path: `Source → [Router D] → [Router E] → Destination` = **2 hops**,  
RIP **always picks the 2-hop path**, even if the 3-hop path is faster (e.g. fiber vs. copper).

> ⚠️ **Important limitation:** RIP only counts hops. It does NOT consider bandwidth, delay, reliability, or link speed.

---

#### 🧭 "Vector" = Next Router (Direction)

Vector means **direction**. Each router only knows:
- Which neighbor to send traffic to next
- How many hops it takes to get to the destination through that neighbor

It does **not** know the full path. Just the next step.

```
Router C's routing table (simplified):
  Network X → via Router B (2 hops)   ← only knows "send it to B"
  Network Y → via Router D (1 hop)    ← only knows "send it to D"
```

---

#### 🏢 "IGP" = Interior Gateway Protocol

IGP means RIP is used **inside one organization's network**.

| Protocol Type | Used For | Examples |
|---|---|---|
| **IGP** | Inside one organization (campus, company) | RIP, OSPF, EIGRP |
| **EGP** | Between different organizations (Internet) | BGP |

> Think of IGP as the hallway routing inside a building, and EGP (BGP) as the roads connecting different cities.

---

## 3. How RIP Learns Routes Step by Step

Here's a concrete walkthrough of how RIP builds its routing table from scratch.

### Network Diagram (Text-Based)

```
[Router R1] ------- [Router R2] ------- [Router R3]
  10.0.12.0/30        10.0.23.0/30
  (G0/0 = .1)         (G0/0 = .1)
  (G0/1 = .2)         (G0/1 = .2)
```

### Step-by-Step Process

**Step 1 — Startup (T=0 seconds)**
Each router only knows its directly connected networks:

```
R1 knows: 10.0.12.0/30 (connected)
R2 knows: 10.0.12.0/30 (connected), 10.0.23.0/30 (connected)
R3 knows: 10.0.23.0/30 (connected)
```

**Step 2 — First Update (T=30 seconds)**
All routers send their tables to their neighbors:

```
R1 sends to R2: "I know 10.0.12.0/30 in 0 hops"
R2 sends to R1: "I know 10.0.12.0/30 in 0 hops, 10.0.23.0/30 in 0 hops"
R2 sends to R3: "I know 10.0.12.0/30 in 0 hops, 10.0.23.0/30 in 0 hops"
R3 sends to R2: "I know 10.0.23.0/30 in 0 hops"
```

**Step 3 — Tables Update (After receiving)**
Each router adds 1 hop to what it received:

```
R1 now knows: 10.0.12.0/30 (direct), 10.0.23.0/30 via R2 (1 hop)
R3 now knows: 10.0.23.0/30 (direct), 10.0.12.0/30 via R2 (1 hop)
```

**Step 4 — Next cycle (T=60 seconds)**
Now R1 and R3 also share what they learned. The knowledge spreads further.

> 📌 This continues until all routers have a complete picture of the network. This is called **convergence**.

---

## 4. Hop Count & Metric

### What is a Metric?

A **metric** is the score a routing protocol uses to rank paths. The **lower the metric, the better the path**.

Different protocols use different metrics:

| Protocol | Metric Used |
|---|---|
| RIP | Hop count |
| OSPF | Cost (based on bandwidth) |
| EIGRP | Composite (bandwidth + delay) |
| BGP | AS Path (number of autonomous systems) |

### RIP's Metric: Hop Count

In RIP, the metric is simply the **number of router jumps** between source and destination.

```
Path 1: A → B → C → D = 3 hops  ← RIP prefers this
Path 2: A → E → F → G → D = 4 hops
Path 3 (via fast fiber): A → H → D = 2 hops  ← RIP picks this (even if A→H is a slow link)
```

### The 15-Hop Limit

> ⚠️ **RIP has a maximum metric of 15 hops.**

| Hop Count | Meaning |
|---|---|
| 1–15 | Valid, reachable route |
| 16 | **Infinity** — the network is considered unreachable |

This hard limit exists to prevent routing loops from running forever. But it also means:

- **RIP cannot be used in large networks** (anything requiring more than 15 router hops)
- It is designed only for **small to medium networks**

---

## 5. RIP Messages — Request & Response

RIP routers communicate using only **two types of messages**:

### Request Message
> *"Hey neighbor, please send me your routing table."*

- Sent when a router first starts up and needs to learn routes quickly
- Also sent when a router needs specific route information

### Response Message
> *"Here is my routing table — all the networks I know and how far they are."*

- Sent in reply to a Request message
- **Also sent automatically every 30 seconds** without being asked

### Automatic Updates (Unsolicited Responses)

RIP routers don't wait to be asked. Every 30 seconds, they **automatically broadcast or multicast their entire routing table** to neighbors.

```
Timeline:
T=0s   → R1 sends full routing table to neighbors
T=30s  → R1 sends full routing table again
T=60s  → R1 sends full routing table again
...and so on forever
```

> ⚠️ This is called **periodic full updates** and is a key RIP characteristic (and weakness — it uses more bandwidth than necessary).

---

## 6. RIPv1 vs RIPv2

There are three versions of RIP. The main focus is **RIPv2**.

| Feature | RIPv1 | RIPv2 | RIPng |
|---|---|---|---|
| IP version | IPv4 | IPv4 | IPv6 |
| Routing type | Classful | Classless | Classless |
| VLSM support | ❌ No | ✅ Yes | ✅ Yes |
| CIDR support | ❌ No | ✅ Yes | ✅ Yes |
| Subnet mask in updates | ❌ No | ✅ Yes | ✅ Yes |
| Update method | Broadcast (255.255.255.255) | Multicast (224.0.0.9) | Multicast (FF02::9) |
| Authentication | ❌ No | ✅ Yes (MD5) | ✅ Yes |
| Status | Obsolete | Current standard | IPv6 only |

---

### RIPv1 — Deep Explanation

RIPv1 is **classful**, meaning it assumes every network uses its default class mask:

| IP Class | Default Mask | Example |
|---|---|---|
| Class A (1–126) | /8 (255.0.0.0) | 10.0.0.0/8 |
| Class B (128–191) | /16 (255.255.0.0) | 172.16.0.0/16 |
| Class C (192–223) | /24 (255.255.255.0) | 192.168.1.0/24 |

**The problem:** RIPv1 does NOT include the subnet mask in its update messages. So when a router receives a route, it guesses the mask based on the class of the IP address.

#### Example of What Goes Wrong

You have two subnets:
```
10.2.1.0/24   (a subnetted Class A)
10.3.5.0/24   (another subnetted Class A)
```

When RIPv1 advertises these, it **strips the /24** and just sends `10.0.0.0`. The receiving router assumes it's `10.0.0.0/8` (the full Class A default mask).

The specific subnet detail is **completely lost**. This breaks routing in any network that uses subnetting — which is virtually every modern network.

---

### RIPv2 — Why It's Better

RIPv2 fixes RIPv1's biggest problems:

1. **Includes subnet mask** in every route advertisement → no guessing, exact subnets preserved
2. **Supports VLSM** (Variable Length Subnet Masking) → you can subnet your networks however you like
3. **Supports CIDR** (Classless Inter-Domain Routing) → route summarization works correctly
4. **Uses multicast** instead of broadcast → only RIP routers receive updates (more efficient)
5. **Supports authentication** → neighbors can verify they're talking to a trusted router

---

## 7. Broadcast vs Multicast

This is one of the key differences between RIPv1 and RIPv2.

### Broadcast (RIPv1) — Address: 255.255.255.255

When a router sends a broadcast:
- **Every single device** on that network segment receives the packet
- This includes PCs, printers, phones, servers — anything on the network
- All those devices must process the packet before deciding to ignore it
- This wastes CPU cycles on every non-router device

```
Router A sends update:
    ┌─────────────────────────────────────┐
    │  Router B ✅ (uses it)              │
    │  Router C ✅ (uses it)              │
    │  PC1      ❌ (receives & discards)  │
    │  PC2      ❌ (receives & discards)  │
    │  Printer  ❌ (receives & discards)  │
    └─────────────────────────────────────┘
```

> 🎙️ **Analogy:** A teacher shouting the lesson to the whole building — students in other classrooms are interrupted even though the message isn't for them.

---

### Multicast (RIPv2) — Address: 224.0.0.9

Multicast sends the packet only to devices that have **joined the multicast group** `224.0.0.9`. Only RIPv2-enabled routers join this group automatically.

```
Router A sends update to 224.0.0.9:
    ┌─────────────────────────────────────┐
    │  Router B ✅ (joined group, gets it) │
    │  Router C ✅ (joined group, gets it) │
    │  PC1      🚫 (never receives it)    │
    │  PC2      🚫 (never receives it)    │
    │  Printer  🚫 (never receives it)    │
    └─────────────────────────────────────┘
```

> 📱 **Analogy:** A WhatsApp group chat — only people in the group see the message. Nobody outside the group is disturbed.

---

## 8. Configuring RIPv2

### Basic RIPv2 Configuration

```cisco
R1# configure terminal
R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# no auto-summary
R1(config-router)# network 10.0.0.0
R1(config-router)# network 192.168.1.0
R1(config-router)# passive-interface GigabitEthernet0/2
R1(config-router)# end
```

### Verification Commands

```cisco
! Check which networks RIP is routing
R1# show ip protocols

! Check the full routing table (look for routes marked with 'R')
R1# show ip route

! Check only RIP routes
R1# show ip route rip

! Check RIP database
R1# show ip rip database

! Troubleshoot RIP updates in real time (careful — verbose output)
R1# debug ip rip
R1# undebug all   ← always turn off debug when done
```

### Reading the Routing Table

When you run `show ip route`, RIP routes are marked with **`R`**:

```
R1# show ip route
...
     10.0.0.0/8 is variably subnetted
R       10.0.23.0/30 [120/1] via 10.0.12.2, 00:00:15, GigabitEthernet0/0
R       10.1.1.0/24  [120/2] via 10.0.12.2, 00:00:15, GigabitEthernet0/0
```

**Reading `[120/1]`:**
| Value | Meaning |
|---|---|
| `R` | Route learned via RIP |
| `120` | Administrative Distance (RIP's default trustworthiness score) |
| `1` | Metric (hop count — 1 hop away) |
| `via 10.0.12.2` | Next-hop router address |
| `00:00:15` | Time since last update |

---

## 9. The `network` Command — How It Really Works

This is one of the most **misunderstood** parts of RIP configuration.

### Common Misconception

❌ *"The `network` command tells the router which networks to advertise."*

### The Truth

✅ *"The `network` command tells the router which **interfaces** to activate RIP on. Those interfaces then advertise their own subnet prefix."*

### How It Works — Step by Step

When you type `network 10.0.0.0`:

1. The router knows `10.0.0.0` is Class A → default mask is **/8**
2. It looks at **all its interfaces** and checks: *does this interface's IP match 10.0.0.0/8?*
3. Any interface with an IP starting with `10.` gets **RIP activated on it**
4. That interface then **advertises its own exact subnet prefix** to neighbors

### Concrete Example

Router R1 has these interfaces:
```
G0/0  → 10.0.12.1/30
G1/0  → 10.0.13.1/30
G2/0  → 192.168.1.1/24
```

You type: `network 10.0.0.0`

```
Check G0/0:  10.0.12.1  →  first 8 bits = 10  →  ✅ MATCH  → RIP activated
Check G1/0:  10.0.13.1  →  first 8 bits = 10  →  ✅ MATCH  → RIP activated
Check G2/0:  192.168.1.1 → first 8 bits = 192 →  ❌ NO MATCH → RIP NOT activated
```

R1 now advertises to neighbors:
```
✅ 10.0.12.0/30  (the real subnet of G0/0)
✅ 10.0.13.0/30  (the real subnet of G1/0)
❌ 10.0.0.0/8    (this is NOT advertised — only the specific subnets are)
```

> 📌 **Key takeaway:** The `network` command is a **selector for interfaces**, not a list of networks to advertise. The interfaces themselves decide what subnet gets shared.

---

## 10. Auto-Summary — Why It Matters

### What is Auto-Summary?

Auto-summary is a RIP feature that **automatically summarizes subnets back to their classful boundary** when advertising across a network boundary.

### With Auto-Summary ON (default in some IOS versions)

Router R1 has two subnets:
- `10.1.1.0/24`
- `10.2.2.0/24`

When R1 advertises these to R2, auto-summary kicks in and says:
> "Both of these are inside the Class A range `10.0.0.0/8`. I'll just advertise the whole `10.0.0.0/8` block."

R2 receives:
```
R  10.0.0.0/8  via R1
```

R2 **loses all subnet detail**. It cannot distinguish between 10.1.1.0 and 10.2.2.0.
This causes incorrect routing — traffic may be sent to the wrong destination.

> 🏠 **Analogy:** Telling someone "I live in Yaoundé" — too vague. They can't find your exact house.

---

### With No Auto-Summary (recommended)

```cisco
R1(config-router)# no auto-summary
```

R1 advertises each subnet separately:
```
R  10.1.1.0/24  via R1  (1 hop)
R  10.2.2.0/24  via R1  (1 hop)
```

R2 knows the exact subnets and can route traffic to the correct destination.

> 🏠 **Analogy:** "I live in Yaoundé, Mvog-Mbi, Street 12, House 5" — precise, no confusion.

### Rule of Thumb

> **Always use `no auto-summary` when configuring RIPv2.** There is almost never a reason to leave auto-summary on.

---

## 11. Passive Interface

### The Problem Passive Interface Solves

By default, when RIP is activated on an interface, the router:
1. Sends routing updates out of that interface
2. Receives routing updates on that interface

But what about interfaces connected to **end devices** (PCs, servers, printers)?

Those devices don't run routing protocols. Sending them RIP updates is:
- A **waste of bandwidth**
- A **security risk** (exposing your routing table to any device on that segment)

### How Passive Interface Works

```cisco
R1(config-router)# passive-interface GigabitEthernet0/2
```

With this command on G0/2:
- ❌ RIP updates are **NOT sent** out of G0/2
- ✅ RIP updates **ARE still received** on G0/2 (if any router sends them)
- ✅ The network connected to G0/2 is **still advertised** to other RIP neighbors via other interfaces

```
                 passive-interface G0/2
                        ↓
[PC1]──[Switch]──[G0/2 of R1]──[G0/0 of R1]──[R2]
         ↑                              ↑
   No updates sent here         Updates sent here
   But 192.168.1.0/24 is still advertised to R2
```

> 📌 **In short:** Passive interface = silent on that port (no outbound updates), but the network is still shared with real routing neighbors.

---

## 12. RIP Timers

RIP uses four timers that control how routes are learned, retained, and removed:

| Timer | Default | Purpose |
|---|---|---|
| **Update** | 30 seconds | How often full routing table is sent to neighbors |
| **Invalid** | 180 seconds | If no update received for this long, route is marked invalid (metric set to 16) |
| **Holddown** | 180 seconds | After a route goes invalid, RIP waits before accepting a new path (prevents flapping) |
| **Flush** | 240 seconds | If still no update after this time, route is completely removed from the table |

### Timer Flow for a Failed Route

```
Route is last heard at T=0s
T=30s   → No update received yet... waiting
T=180s  → Route marked INVALID (metric = 16 = unreachable). Holddown starts.
T=180s–360s → Holddown: RIP will NOT accept a worse route for this network
T=240s  → Route FLUSHED from routing table entirely
```

You can modify timers with:
```cisco
R1(config-router)# timers basic 30 180 180 240
```
> ⚠️ All routers in the network must have the same timer values or routing will break.

---

## 13. RIP Limitations & When NOT to Use It

### RIP's Weaknesses

| Limitation | Why It's a Problem |
|---|---|
| **Max 15 hops** | Cannot be used in large networks |
| **Slow convergence** | 30-second update interval means route failures can take minutes to fix |
| **No bandwidth awareness** | Picks a slow 2-hop path over a fast 5-hop path |
| **Full table every 30s** | Even if nothing changed, the whole table is sent — wastes bandwidth |
| **Routing loops possible** | Without careful configuration, loops can form during convergence |
| **RIPv1 has no authentication** | Anyone can inject fake routes |

### When to Use RIP

✅ Small lab environments and learning/testing  
✅ Very small networks (under 15 hops)  
✅ Networks where simplicity matters more than efficiency  
✅ Legacy environments that haven't migrated yet  

### When NOT to Use RIP

❌ Large enterprise networks  
❌ Networks where fast convergence is critical  
❌ Networks with unequal-speed links (RIP ignores bandwidth)  
❌ Any network with more than 15 router hops  

> 💡 In modern networks, **OSPF** or **EIGRP** are almost always preferred over RIP. RIP is primarily taught because it's the simplest routing protocol — it makes a great learning foundation before moving to more complex protocols.

---

## 14. Quick Reference Cheat Sheet

### Core Facts

| Topic | Value / Answer |
|---|---|
| Full name | Routing Information Protocol |
| Protocol type | Distance Vector IGP |
| Metric | Hop count |
| Maximum metric | 15 (16 = infinity / unreachable) |
| Update interval | Every 30 seconds |
| Update type | Full routing table (periodic) |
| RIPv1 update address | Broadcast → 255.255.255.255 |
| RIPv2 update address | Multicast → 224.0.0.9 |
| RIPng update address | Multicast → FF02::9 (IPv6) |
| Administrative Distance | 120 |

### RIPv1 vs RIPv2 — One-Line Summary

> **RIPv1** = classful, no subnet mask, broadcasts, no VLSM, obsolete.  
> **RIPv2** = classless, includes subnet mask, multicasts, supports VLSM/CIDR, current standard.

### Config Checklist

```cisco
router rip
 version 2               ← Always specify version 2
 no auto-summary         ← Always disable auto-summary
 network [network-addr]  ← Activates RIP on matching interfaces (classful range)
 passive-interface [int] ← Silence RIP on end-device ports
```

### Key Concepts — One Line Each

- **Routing by rumor** → routers only know what neighbors tell them
- **Hop count** → the only metric RIP uses to choose paths
- **Vector** → each router only knows the *next hop*, not the full path
- **IGP** → used inside one organization's network (not between ISPs)
- **network command** → activates RIP on interfaces, not a list of networks to advertise
- **no auto-summary** → keeps exact subnet details, prevents routing errors
- **passive-interface** → stops outbound updates on a port, but the network is still advertised
- **15-hop limit** → routes beyond 15 hops are considered unreachable (metric = 16)

---

*Study notes compiled from CCNA networking fundamentals. For corrections or additions, feel free to open an issue or pull request.*
