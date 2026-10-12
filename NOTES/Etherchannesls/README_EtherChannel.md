# CCNA – EtherChannel

Study notes on EtherChannel (link aggregation): what it is, the problem it solves, how traffic is load balanced, and the ways to form one.

**File:** `CCNA_EtherChannel_Notes.docx`

---

## What's Inside

1. **What is EtherChannel?** – Layer 2 vs Layer 3 EtherChannel, access vs distribution switch
2. **The problem it solves** – oversubscription on a congested uplink (ASW1 with 40 hosts to DSW1)
3. **How EtherChannel works** – the group acts as one logical interface, why it does not cause a Layer 2 loop
4. **Load balancing** – per-flow balancing and the inputs the algorithm can use
5. **Ways to form an EtherChannel** – PAgP, LACP, static
6. **How many interfaces?** – 8 per group, 16 with LACP (8 active, 8 standby)
7. **Member interface requirements** – settings that must match
8. **Layer 3 EtherChannel** – routed ports and why modern designs favour them
9. **Still to cover** – configuration

Three diagrams are embedded in the notes: the oversubscribed uplink, the EtherChannel circled as one logical link, and per-flow load balancing.

---

## Key Points

- An EtherChannel groups multiple physical interfaces into **one logical interface**.
- **STP treats the group as a single interface**, so all member links forward and there is no Layer 2 loop.
- Load balancing is **per flow**: frames in the same flow always use the same physical interface, which avoids out-of-order delivery.
- **LACP** is the preferred method, and the one in the CCNA.

### Formation methods

| Method | Standard? | Summary |
| --- | --- | --- |
| PAgP | Cisco only | Dynamically negotiates and maintains the EtherChannel |
| LACP | Industry standard (IEEE) | Same job as PAgP, works across vendors, preferred |
| Static | No protocol | Manually configured, usually avoided |

### Load balancing inputs

- Source MAC
- Destination MAC
- Source and destination MAC
- Source IP and destination IP

### Member interfaces must match

> Same **duplex**, same **speed**, same **switchport mode** (access or trunk), and if trunk, the same **allowed VLANs** and **native VLAN**. An interface that does not match is **excluded** from the EtherChannel.

---

## Changes Made While Formatting

- The original was a lightly edited video transcript with many typos. It was reorganised into numbered sections and bullets with the meaning kept.
- Repeated passages (flow explanation, PAgP/LACP descriptions) were merged.
- The question about why DSW1 does not send the frame back out of the other member links is now a callout in section 3.
- The three diagrams were added in the sections they relate to.
- Configuration was named as a topic in the original but had no content, so it is listed under **Still to cover**.

---

## Related Notes

- CCNA STP Part 2: Port States and Timers (`CCNA_STP_Part2_Notes.docx`)
- CCNA CDP and LLDP (`CCNA_CDP_and_LLDP_Notes.docx`)
