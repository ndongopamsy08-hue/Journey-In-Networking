# CCNA – CDP and LLDP

Study notes on the two Layer 2 discovery protocols covered in the CCNA: **CDP** (Cisco proprietary) and **LLDP** (IEEE 802.1AB).

**File:** `CCNA_CDP_and_LLDP_Notes.docx`

---

## What's Inside

1. **Layer 2 discovery protocols** – what they are, what they share, when to use CDP vs LLDP, and the security consideration
2. **How the protocols work** – the R1 / SW1 example, with a diagram of the exchange
3. **CDP (Cisco Discovery Protocol)** – basics, messages, timers
4. **LLDP (Link Layer Discovery Protocol)** – basics, messages, timers
5. **CDP vs LLDP** – side-by-side comparison table and a CCNA tip

---

## Key Points

- Both protocols discover **directly connected neighbours** and share host name, IP address, device type, and so on.
- Received messages are **processed and discarded**, never forwarded to other devices, even though they use multicast MAC addresses.
- Both can run on the same device at the same time.
- Because they reveal network details, they can be a **security risk**. The administrator decides whether to enable them.

### Quick comparison

| Feature | CDP | LLDP |
| --- | --- | --- |
| Type | Cisco proprietary | Industry standard (IEEE 802.1AB) |
| Best used when | Network is all Cisco | Mixed vendors |
| Default on Cisco devices | Enabled | Usually disabled |
| Multicast MAC address | `0100.0ccc.cccc` | `0180.c200.000e` |
| Send interval | 60 seconds | 30 seconds |
| Hold time (default) | 180 seconds | Not covered in these notes |

> **Remember:** 60 s / 180 s for CDP, 30 s for LLDP, and LLDP must be enabled manually on Cisco devices.

---

## Changes Made While Formatting

- **CDP hold time:** the original notes said both 80 seconds and 180 seconds. The default is **180 seconds**, so only that value is used.
- **SW1 and IP addresses:** the original sentence was garbled. It now says SW1 sends no IP address because its interfaces do not have one (a Layer 2 switch).
- Repeated and run-on lines (security risk, multicast explanation) were merged into single bullets.
- LLDP hold time is not in the original notes, so it is marked as not covered rather than filled in.

---

## Related Notes

- CCNA STP Part 2: Port States and Timers (`CCNA_STP_Part2_Notes.docx`)
