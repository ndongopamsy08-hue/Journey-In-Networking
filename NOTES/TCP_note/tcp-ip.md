# TCP/IP Model — Complete Notes

---

## 1. What Is a Protocol?

A **protocol** is a set of rules and regulations that devices follow so that communication can take place. It defines *how data should be formatted, transmitted, received, and verified* between devices over a network.

> Think of protocols as the **common language** that devices use to understand each other.

### Why Standard Protocols Matter

In the early days of networking, vendors each created their own proprietary communication functions. This made it nearly impossible for devices from different manufacturers to talk to each other.

| area              |                             Approach                         |               Result                        |
| Early networking  |            Each vendor had its own rules                     |         Devices were incompatible
| Modern networking | Universal standards (TCP/IP,  HTTP, Wi-Fi, Bluetooth)        |   Any device can communicate with any other |

A **standard** is an agreed-upon specification — vendor-neutral — that describes exactly how a protocol or technology should work. Most networking standards are developed by **independent standards organizations** (e.g., IEEE, IETF) with participation from engineers across many companies, not by a single vendor.

---📡 Summary: Standards and Prprotocol is a set of rules that defines how devices communicate (e.g., TCP, HTTP, WPA2).

A standard is an official agreement created by organizations (IEEE, ISO, IETF, ITU) that specifies which protocols must be used and how, ensuring compatibility across devices.

Each standard may include one or multiple protocols.

Example: IEEE 802.11 (Wi‑Fi)
Standard: IEEE 802.11

Protocols inside it:

WPA2/WPA3 → authentication & encryption

MAC layer rules → manage access to the wireless medium

Frame formats → define Wi‑Fi packet structure

👉 Takeaway:
Protocols are the rules of communication, while standards are the official frameworks that organize and enforce those rules so all devices can work together.

## 2. Why Use a Layered Model?
 
A network has many different jobs to move data from one computer to another:
- Physical transmission of signals(Network Access Layer  )
- Local delivery within a LAN(Network Access Layer  )
- Routing traffic between networks(internet layer )
- End-to-end conversations between applications(application layer )

A **layered model** groups related jobs into layers. Each layer:
- Has a **specific, focused role**
- Uses the **services of the layer below** it
- Provides **services to the layer above** it
- Works **independently** — what happens inside one layer does not change the job of another layer

> Analogy: Changing the *content* of a letter doesn't change how the delivery truck works.

---

## 3. The TCP/IP Model Layers

```
┌─────────────────────────────┐
│      Application Layer      │  ← Layer 4
├─────────────────────────────┤
│      Transport Layer        │  ← Layer 3
├─────────────────────────────┤
│      Internet Layer         │  ← Layer 2
├─────────────────────────────┤
│   Network Access Layer      │  ← Layer 1
│  (Data Link + Physical)     │
└─────────────────────────────┘
```

---

### Layer 4 — Application Layer

The **Application Layer** is where applications (browsers, email clients, file transfer tools) interact directly with the network using specific protocols.

- It is the point where **network communications meet user applications**
- Common protocols: **HTTP/HTTPS** (web), **SMTP/IMAP** (email), **FTP** (file transfer), **DNS** (name resolution)

---

### Layer 3 — Transport Layer

The **Transport Layer** is responsible for **end-to-end communication between applications** on different devices. It ensures that data sent by one application arrives correctly at the right application on another device.

Uses **port numbers** to identify which process/application should receive the data.

#### Analogy: The Post Office Worker
1. Puts your data into the right envelope
2. Labels it with the correct port number (like a room number)
3. Splits large data into smaller segments if needed
4. Reassembles segments in the correct order at the destination
5. Requests retransmission of any lost pieces

#### Key Protocols
| Protocol        |               Full Name                      |                   Characteristics                  |
| **TCP**         |               Transmission Control Protocol  | Reliable, ordered, error-checked delivery       |
| **UDP**         |               User Datagram Protocol         | Fast, connectionless, no guarantee of delivery  |

> **Note:** The Transport Layer runs mainly on the two **communicating hosts** (e.g., PC1 and Server1). Routers generally operate at Layer 2 (Internet/IP layer) and do not process Transport Layer headers.

---

### Layer 2 — Internet Layer

The **Internet Layer** is responsible for **addressing and routing**. It ensures that data packets know where to go and how to get there across multiple networks.

- Uses **IP addresses** to identify source and destination hosts
- When PC1 sends a message to Server1, it addresses the packet to Server1's IP address   
- **Routers** operate at this layer, reading the destination IP address to forward packets toward their final destination

#### Key Protocols
| Protocol | Purpose |
|----------|---------|
| **IPv4 / IPv6** | Addressing and packet delivery |
| **ICMP** | Error reporting and diagnostics (e.g., `ping`) |

---

### Layer 1 — Network Access Layer (Data Link + Physical)

This layer combines two responsibilities:

**Data Link (LAN) sub-layer:**
- Provides **hop-to-hop delivery** within a local network
- Uses **MAC addresses** and **switches**
- A *hop* is one step along the path between two devices (e.g., from one router to the next)

**Physical sub-layer:**
- Sends bits as **electrical, optical, or radio signals** over a physical medium (cables, fiber, air)

---

## 4. Encapsulation & Decapsulation

When data travels across a network, each layer **wraps the data with its own header** (and sometimes a trailer). This process is called **encapsulation**. At the destination, each layer **removes its header** in reverse — this is called **decapsulation**.

---

### Encapsulation (Sending Side)

As data moves **down** through the layers, each layer adds its own header:

```
Application Layer   →  [  DATA  ]
                              ↓ adds Transport header
Transport Layer     →  [ TH | DATA ]          (Segment)
                              ↓ adds Internet header
Internet Layer      →  [ IH | TH | DATA ]     (Packet)
                              ↓ adds Network Access header + trailer
Network Access Layer→  [ NH | IH | TH | DATA | T ]  (Frame)
                              ↓ converted to bits
Physical Layer      →  101010110001...          (Bits)
```

| Layer           |                 PDU Name                   |               Header Added                             |
| Application     |       Data / Message                       | —                                                      |
| Transport       |         Segment                            | Source & destination **port numbers**                  |
| Internet        |           Packet                           | Source & destination **IP addresses**                  |
| Network Access  |       Frame                                | Source & destination **MAC addresses** + trailer (FCS) |
| Physical        |             Bits                           |                  Converted to signals                  |

> **PDU** = Protocol Data Unit — the name of the data at each layer.

#### Step-by-step (Sender — PC1 → Server1)

1. **Application Layer** — The browser generates an HTTP request (the raw data/message).
2. **Transport Layer** — TCP wraps the data into a **segment**, adding source port (e.g., 54321) and destination port (e.g., 80 for HTTP).
3. **Internet Layer** — IP wraps the segment into a **packet**, adding PC1's IP address as source and Server1's IP address as destination.
4. **Network Access Layer** — The NIC wraps the packet into a **frame**, adding MAC addresses for the current hop (e.g., PC1's MAC → Router's MAC) and a Frame Check Sequence (FCS) trailer for error detection.
5. **Physical Layer** — The frame is converted into **bits** and sent as electrical/optical/radio signals over the medium.

---

### Decapsulation (Receiving Side)

As data moves **up** through the layers at the destination, each layer **strips off its header**:

```
Physical Layer      →  101010110001...          (Bits received)
                              ↓ reconstructs frame
Network Access Layer→  [ NH | IH | TH | DATA | T ]
                              ↓ strips frame header/trailer
Internet Layer      →  [ IH | TH | DATA ]
                              ↓ strips IP header
Transport Layer     →  [ TH | DATA ]
                              ↓ strips TCP header, reassembles segments
Application Layer   →  [  DATA  ]               (delivered to app)
```

#### Step-by-step (Receiver — Server1)

1. **Physical Layer** — Receives the raw signal and converts it back into bits.
2. **Network Access Layer** — Reconstructs the frame, checks the FCS for errors, reads the destination MAC address to confirm it's for this device, then strips the frame header/trailer and passes the packet up.
3. **Internet Layer** — Reads the destination IP address to confirm it matches, strips the IP header, and passes the segment up.
4. **Transport Layer** — Reads the destination port number (e.g., 80), reassembles segments in order (TCP), strips the transport header, and delivers the data to the correct application process.
5. **Application Layer** — The web server application receives the raw HTTP request and processes it.

---

### Encapsulation at Intermediate Devices (Routers)

Routers perform a **partial decapsulation/re-encapsulation** at every hop:

```
PC1 → [Frame: MAC_PC1→MAC_R1 | Packet: IP_PC1→IP_Srv1 | Segment | Data]
           ↓ Router R1 strips frame header, reads IP address
R1  → [Frame: MAC_R1→MAC_R2 | Packet: IP_PC1→IP_Srv1 | Segment | Data]
           ↓ Router R2 strips frame header, reads IP address
R2  → [Frame: MAC_R2→MAC_Srv1 | Packet: IP_PC1→IP_Srv1 | Segment | Data]
           ↓ Server1 receives
```

