# CCNA – TCP/IP Model

Study notes on protocols, standards, and the five-layer TCP/IP model, including encapsulation and decapsulation.

**File:** `CCNA_TCPIP_Model_Notes.docx`

---

## What's Inside

1. **Protocols and standards** – what a protocol is, what functions it defines, proprietary vs standard
2. **Why use a layered model?** – grouping related jobs into layers
3. **Building a model: the post office analogy** – letter, envelope, post offices, mapped to network layers
4. **The TCP/IP layers** – Application, Transport, Internet, Local network, Physical
5. **Layer interaction** – adjacent-layer and same-layer interaction
6. **Encapsulation** – headers added as data moves down the stack
7. **Decapsulation** – the same process in reverse

Seven diagrams from your original notes are embedded in the sections they relate to (post office analogy, the five layers between PC1 and SRV1, hops, physical layer examples, adjacent-layer and same-layer interaction, encapsulation).

---

## Key Points

- A **protocol** is a set of rules for how devices communicate. A **standard** is an agreed specification that different manufacturers follow so their devices can work together.
- Each layer has its own job, uses the services of the layer below, and serves the layer above.
- A **port** is a number that identifies a process on a host (80 for the web server, 21 for the file server in the example).
- A **hop** is one step between hosts or routers. **Switches do not count** as hops.

### The five layers

| Layer | Role | Key identifier |
| --- | --- | --- |
| Application | Creates and interprets the data | Protocols such as HTTP |
| Transport (L4) | End-to-end between application processes | Port numbers (TCP, UDP) |
| Internet (L3) | End-to-end between hosts across networks | IP addresses (IP, ICMP), routers |
| Local network (L2) | Hop-to-hop delivery in a local network | MAC addresses, switches |
| Physical (L1) | Sends and receives bits as signals | Cables, Wi-Fi, NICs |

### Encapsulation

> Application data, then a **Layer 4 header** (ports), then a **Layer 3 header** (IP addresses), then a **Layer 2 header and trailer** (MAC addresses and error check), then bits on the wire. Decapsulation removes them in reverse order.

---

## Changes Made While Formatting

- The original text was a lightly edited video transcript with many typos and a few broken sentences. It was reorganised into numbered sections and bullets with the meaning kept.
- Empty bullets and stray characters were removed.
- The post office analogy and the PC1 to SRV1 walkthrough were each split into short bullets and tables.
- A table mapping the analogy layers (content, recipient, address, local delivery) to network layers (data, port, IP address, MAC address) was added in section 3.
- One diagram (hops) had a cut-off line of text at the bottom, which was cropped out.
- The Gmail and post office worker examples were kept under the Transport layer.

---

## Related Notes

- CCNA STP Part 2: Port States and Timers (`CCNA_STP_Part2_Notes.docx`)
- CCNA CDP and LLDP (`CCNA_CDP_and_LLDP_Notes.docx`)
- CCNA EtherChannel (`CCNA_EtherChannel_Notes.docx`)
