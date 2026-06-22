# Ethernet LAN Switching — Lesson Notes (Part 2)

## What This Lesson Covers

This lesson is **Part 2** of a series on Ethernet LAN Switching (from Jeremy's IT Lab / CCNA).  
It builds on Part 1's introduction to the Ethernet frame, then explains **how devices actually find each other on a local network** using ARP, how switches learn MAC addresses, and how to use Ping.

---

## Topics Covered

### 1. Ethernet Frame Size Rules
- The Ethernet frame consists of: **Preamble | SFD | Destination MAC | Source MAC | Type | Payload | FCS**
- Preamble (7 bytes) and SFD (1 byte) are **not** counted as part of the header
- **Minimum frame size = 64 bytes**
- Minimum payload = 46 bytes → if the packet is smaller, **padding bytes** are added
  - Example: 34-byte packet + 12-byte padding = 46 bytes

---

### 2. Network Topology Used
The lesson uses a small LAN with 4 PCs and 2 switches:

| Device | MAC Address      | IP Address    | Connected To  |
|--------|-----------------|---------------|---------------|
| PC1    | 0C2F.B011.9D00  | 192.168.1.1   | SW1 (G0/0)    |
| PC2    | 0C2F.B084.6200  | 192.168.1.2   | SW1 (G0/1)    |
| PC3    | 0C2F.B06A.3900  | 192.168.1.3   | SW2 (G0/0)    |
| PC4    | 0C2F.B01E.0A00  | 192.168.1.4   | SW2 (G0/1)    |
| SW1    | —               | —             | SW2 via G0/2  |

- All devices are on network **192.168.1.0/24**
- All MAC addresses share the same OUI (same manufacturer)

---

### 3. IP + MAC Encapsulation
When a device sends data:
1. It creates an **IP packet** with source & destination IP addresses
2. That IP packet is **encapsulated in an Ethernet frame** with source & destination MAC addresses
3. **You type an IP address** — the MAC is resolved automatically by ARP

---

### 4. ARP — Address Resolution Protocol
ARP finds the MAC address of a device given its IP address.

#### ARP Request (Broadcast)
- Sent when the source device does **not** know the destination's MAC
- Destination MAC in the frame = **FFFF.FFFF.FFFF** (broadcast — all devices receive it)
- Switches **flood** broadcast frames out all ports except the incoming one
- Along the way, switches **learn** the sender's MAC → interface mapping

#### ARP Reply (Unicast)
- The target device replies with **its MAC address**
- The reply is **unicast** — sent directly back to the requester (not broadcast)
- Switches learn MAC addresses from the reply too

#### ARP Process (PC1 → PC3)
```
1. PC1 sends ARP Request (broadcast FFFF.FFFF.FFFF)
2. SW1 floods → learns PC1 MAC on G0/0
3. SW2 receives → floods → PC3 and PC4 get it
4. SW2 learns PC1 MAC is reachable via G0/2
5. PC3 recognizes its own IP → sends ARP Reply (unicast to PC1)
6. Reply travels back: PC3 → SW2 → SW1 → PC1
7. PC1 now knows PC3's MAC and can send data directly
```

---

### 5. Ping
- Network utility used to **test reachability** between devices
- Measures **round-trip time (RTT)**
- Uses **ICMP** (Internet Control Message Protocol):
  - `ICMP Echo Request` — sent by source
  - `ICMP Echo Reply` — returned by destination

---

### 6. MAC Address Table — Aging & Clearing

#### Aging
- Switches remove dynamic MAC entries after **300 seconds** (5 minutes) of inactivity
- This is called the **aging timer**

#### Cisco CLI Commands

| Action | Command |
|--------|---------|
| View MAC address table | `SW1# show mac address-table` |
| Clear all dynamic entries | `SW1# clear mac address-table dynamic` |
| Clear a specific MAC address | `SW1# clear mac address-table dynamic address 0c2f.b011.9d00` |
| Clear all entries on an interface | `SW1# clear mac address-table dynamic interface Gi0/0` |

---

## Key Takeaways

| # | Concept |
|---|---------|
| 1 | Minimum Ethernet frame = **64 bytes**; padding added if payload < 46 bytes |
| 2 | Devices communicate by IP; MACs are resolved via **ARP** |
| 3 | ARP Request = **broadcast** (`FFFF.FFFF.FFFF`) |
| 4 | ARP Reply = **unicast** (directly to requester) |
| 5 | Switches learn MACs dynamically from incoming frames |
| 6 | Unknown/broadcast frames are **flooded**; known unicast frames are **forwarded** |
| 7 | Ping uses ICMP to verify connectivity |
| 8 | MAC entries age out after **300 seconds** by default |

---

## Files in This Repo

| File | Description |
|------|-------------|
| `Ethernet_LAN_Switching_Notes.docx` | Full lesson notes with all screenshots embedded |
| `README_Ethernet_LAN_Switching.md` | This file — lesson summary and quick reference |

---

*Source: Jeremy's IT Lab — CCNA Course, Ethernet LAN Switching Part 2*
