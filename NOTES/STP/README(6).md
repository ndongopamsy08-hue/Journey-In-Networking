# CCNA – STP Part 2: Port States and Timers

Study notes on Spanning Tree Protocol (STP) for the CCNA exam. This document is Part 2 of the STP series and covers port states and timers.

**File:** `CCNA_STP_Part2_Notes.docx`

---

## Part 2 Outline

| Topic | Status |
| --- | --- |
| STP port states / timers | Covered |
| STP BPDU | To cover |
| STP optional features | To cover |
| STP configuration | To cover |

---

## What's Inside

1. **STP port states** – the four states and which ports end up in each
2. **What each state does** – Blocking, Listening, Learning, Forwarding
3. **Comparison table** – BPDUs, MAC learning, user traffic and duration per state
4. **Timers (defaults)** – Hello, Forward delay, Max Age
5. **Three-switch example** – how root bridge, root ports, designated and non-designated ports are chosen
6. **Q&A / clarifications** – listening vs. learning, why MAC learning happens before forwarding, why ports take time to turn green
7. **Still to cover** – BPDU, optional features, configuration commands

---

## Key Points

### Port state progression

```
Blocking → Listening → Learning → Forwarding
 (stable)   (15 s)      (15 s)     (stable)
```

**Memory trick:** B → L → L → F

- **Root and designated ports** end up in the stable **forwarding** state.
- **Non-designated ports** stay in the stable **blocking** state.
- **Listening** and **learning** are transitional states, used when an interface comes up or a topology change moves a port to forwarding.

### State comparison

| State | Type | Sends/receives BPDUs | Learns MACs | Forwards user traffic | Duration |
| --- | --- | --- | --- | --- | --- |
| Blocking | Stable | Receives only | No | No | Until topology changes |
| Listening | Transitional | Yes | No | No | 15 s |
| Learning | Transitional | Yes | **Yes** | No | 15 s |
| Forwarding | Stable | Yes | Yes | Yes | Until topology changes |

### Default timers

| Timer | Default | Purpose |
| --- | --- | --- |
| Hello | 2 s | How often BPDUs are sent |
| Forward delay | 15 s | Time in listening, and again in learning (30 s total) |
| Max Age | 20 s | How long a switch keeps a BPDU before treating it as expired |

> **Common mix-up:** The 20 s value is the **Max Age** timer, not a blocking-to-listening delay. The normal transition is **15 s listening + 15 s learning**.

---

## Three-Switch Example (Summary)

SW1, SW2 and SW3 form a triangle, all with priority 32768 and MACs AAAA, BBBB and CCCC.

- **SW1** becomes the root bridge (priorities tie, lowest MAC wins), so all its active ports are designated.
- **SW2 and SW3** each choose a root port toward SW1.
- On the **SW2–SW3 link**, one side becomes designated and the other becomes non-designated (blocking), which breaks the loop.
- STP runs **one calculation per VLAN**.

---

## CCNA Tips

- **PortFast** on ports facing end devices lets them reach forwarding almost immediately.
- **Rapid PVST+** converges much faster than traditional STP and is common in modern Cisco networks.

---

## Related Notes

- STP Part 1
- STP Part 2 (this document)
- STP Part 3 – BPDU, optional features and configuration (planned)
