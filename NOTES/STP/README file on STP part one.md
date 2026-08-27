# STP — Spanning Tree Protocol

> Notes reorganized for a clearer read-through — content unchanged, just grouped by topic so a new reader can follow the logic in order.

## 1. Redundancy in Networks

- Redundancy is an essential part of network design.
- **Redundancy = backup paths** in a network.
- It means the network can **keep working** even if one link or device fails.
- Modern networks are expected to run 24/7/365. Even a short downtime can be disastrous for a business.
- A network that is not redundant is not acceptable — it is rejected by the enterprise.
- Most PCs only have a single network interface card (NIC), so they can only be plugged into a single switch. Important servers typically have multiple NICs so they can be plugged into multiple switches for redundancy.

![Non-redundant network](word/media/image1.png)
*A network that is NOT redundant — if one part fails to transmit data, the whole network has a problem.*

![Redundant network](word/media/image2.png)
*A good, redundant network design — if one part fails, there is another road that can still transmit the data.*

## 2. The Problem Redundancy Creates: Loops

### MAC Address Flapping
- Network congestion is not the only problem: each time a frame arrives on a switch port, the switch uses the source MAC address field to learn the MAC address and update its MAC address table.
- When frames with the same source MAC address repeatedly arrive on different interfaces, the switch keeps updating that interface in its MAC address table — this is known as **MAC address flapping**.

### Broadcast Storms
- **TTL (time to live)** is like an expiration counter for packets — it makes sure they don't live forever. The Ethernet header does not have a TTL field, so broadcast frames will loop around the network indefinitely.
- If enough of these loop, the network becomes too congested for legitimate traffic to use — this is called a **broadcast storm**.
- A broadcast storm occurs when the network fills up with loopy broadcast frames that no regular traffic can get through.

### Worked Example
When SW1 receives the frame it floods it to all interfaces (SW2 and SW3). Both receive a copy and flood it out their own interfaces, until it reaches PC2. PC2 replies with a unicast ARP reply, which is not enough to stop the problem — even though PC2 received the ARP request and sent its reply, the original broadcast frames are still looping on the network, and the switches keep flooding them.

![](word/media/image3.png)
![](word/media/image4.png)
![](word/media/image5.png)
![](word/media/image6.png)
![](word/media/image7.png)

## 3. What STP Does

- Switches from all vendors run STP by default.
- STP prevents Layer 2 loops by placing redundant ports in a blocking state — essentially disabling the interface.
- That interface acts as a backup and can enter a forwarding state if a currently-forwarding interface fails.
- **Forwarding state**: the interface behaves normally — it sends and receives all normal traffic.
- **Blocking state**: the interface only sends or receives STP messages, called BPDUs (Bridge Protocol Data Units).
- By selecting which ports are forwarding and which are blocking, STP creates a single path to/from each point in the network — this is what prevents Layer 2 loops.
- There is a set process STP uses to determine which ports should be forwarding and which should be blocking.

### BPDUs (Bridge Protocol Data Units)
- STP-enabled switches send and receive hello BPDUs out of all interfaces. The default timer is 2 seconds — a switch sends a hello BPDU out of every interface once every 2 seconds.
- STP doesn't "physically" send BPDUs — the switch does. STP is the reason the switch sends them every 2 seconds.
- If a switch receives a hello BPDU on an interface, it knows that interface connects to another switch (routers, PCs, etc. don't run STP, so they don't send BPDUs).

## 4. Root Bridge Election

- The switch with the lowest Bridge ID becomes the root bridge.
- All ports on the root bridge are put in a forwarding state, and every other switch in the topology must have a path to reach the root bridge.

### The Bridge ID
The Bridge ID is the unique identifier of a switch in the Spanning Tree Protocol.

The default bridge priority is 32768 on all switches, so by default the MAC address is used as the tie-breaker (the lowest MAC address becomes the root bridge).

**The bridge priority is compared first. If they tie, the MAC address is then compared.**

### PVST (Per-VLAN Spanning Tree)
- Cisco switches use a version of STP called PVST (Per-VLAN Spanning Tree). PVST runs a separate STP instance in each VLAN, so in each VLAN different interfaces can be forwarding/blocking.
- This means STP can run independently in different VLANs.
- The bridge priority is made up of two fields: the bridge priority and the extended system ID (VLAN ID) — so each VLAN has its own bridge priority.

### Bridge Priority + Extended System ID
The bridge priority + extended system ID together form a single field, the Bridge ID. The extended system ID is fixed and cannot be changed, because it is determined by the VLAN ID.

![](word/media/image8.png)

**Because of this, you can only change the total bridge priority (bridge priority + extended system ID) in units of 4096** — the value of the least significant bit of the bridge priority.

### Should You Change the Bridge Priority?
- **Yes** — it's advised to change the bridge priority on the switch you want as the root bridge.
- If you don't, STP will choose automatically based on MAC address, which might not be the best switch.
- **Best practice:** manually set the priority so the root bridge is predictable and stable.

### Why Change It?
- In enterprise networks, you usually want a specific, powerful, central switch to be the root bridge.
- To force that, you lower its bridge priority (e.g., set it to 4,096 or 0).
- Lower priority = higher chance of winning the root bridge election.

### Relationship Between Total, Priority, and VLAN
- The Total Bridge Priority field = Bridge Priority (configurable, multiples of 4096) + VLAN ID (automatic).
- So if you already know the Total and the VLAN ID, you can find the configured Bridge Priority:

**Bridge Priority = Total − VLAN ID**

## 5. Designated Ports

- All interfaces on the root bridge are designated ports, and designated ports are in a forwarding state.
- When a switch is powered on, it assumes it is the root bridge.
- Once the topology has converged and all switches agree on the root bridge, only the root bridge sends BPDUs.
- Other switches in the network forward these BPDUs but do not generate their own.
- Only the root bridge generates hello BPDUs. Other switches forward those BPDUs out their designated ports, so the whole network stays loop-free.

A **Designated Port** is the port chosen to forward traffic toward the root bridge on a given network segment. It's also the port on a switch responsible for sending BPDUs onto that segment.

- On every LAN segment, only one port is chosen as the designated port.
- That port is always in the forwarding state — meaning it passes normal traffic and also originates BPDUs for that segment.

![](word/media/image9.png)

## 6. Root Ports and Root Cost

- Each remaining (non-root) switch selects one of its interfaces to be its root port — the interface with the lowest root cost becomes the root port.
- Root ports are also in the forwarding state.

### Root Port vs. Other Links
- On a non-root switch, STP chooses one root port: the port with the lowest cost path to the root bridge.
- That root port stays in the forwarding state.
- Any other port on the same switch that leads toward the root bridge but is not the best path is put into a blocking state, to prevent loops.

### Root Cost
**Root Cost = the total path cost from a switch to the root bridge.** Lower root cost = better path. That's how STP decides which port becomes the root port.

**The root cost is the total cost of the outgoing interfaces along the path to the root bridge.**

- **Important:** you don't count the cost of the receiving interface — only the sending (outgoing) interface.
- Each remaining switch selects one interface to be its root port (forwarding state). The port directly across from the root port (on the neighbor) is always a designated port.

### Root Port Selection — Tie-breakers
- Lowest root cost.
- Lowest neighbor bridge ID — used when two paths have the same cost to the root bridge; STP compares the neighbor's Bridge ID first, then the Port ID.

![](word/media/image10.png)
*If there is a tie in root cost, the switch selects the interface connected to the neighbor with the lowest bridge ID. (Switch 1 is the root bridge here, so it has a cost of 0 on all its interfaces — remember, only the cost of the outgoing/sending interface counts, not the receiving one.)*

- STP tie-break order on a port: port priority + port number.

Every collision domain has a single STP designated port.

![](word/media/image11.png)

## 7. Summary

### 1. Root Bridge Election
One switch is elected as the root bridge. All ports on the root bridge are designated ports (forwarding state). Root bridge selection is based on the lowest Bridge ID (priority = VLAN ID + priority bit, then MAC address).

### 2. Root Port Selection
Each remaining switch selects one of its interfaces to be the root port (forwarding state). The port directly across from the root port is also a designated port. Root port selection is based on:
1. Lowest root cost
2. Lowest neighbor bridge ID
3. Lowest neighbor port ID

### 3. Designated Port Selection
Each remaining collision domain selects one interface to be the designated port (forwarding state); the other port in that collision domain becomes the non-designated (blocking) port. Selection is based on:
1. The interface on the switch with the lowest root cost
2. The interface on the switch with the lowest Bridge ID

This means that if two interfaces have the same root cost, you compare their Bridge ID to decide.

![](word/media/image11.png)

**Edge case:** if two switches have two connections between them, so both root cost and neighbor Bridge ID are the same, the interface connected to the neighbor's lowest Port ID becomes the root port.

![](word/media/image12.png)
