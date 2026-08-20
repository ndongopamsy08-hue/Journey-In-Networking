# README — Subnetting Study Notes

This file is a companion guide to **Subnetting Notes** (the source document). It explains what's in the notes, gives you a quick-reference cheat sheet, and walks through a real-world scenario that shows *why* subnetting — and specifically VLSM — actually matters outside of a textbook.

---

## What's in the Source Notes

| Section | Covers |
|---|---|
| CIDR (Classless Inter-Domain Routing) | 32-bit IPv4 structure, address classes A–E, who assigns blocks (IANA) |
| Special Prefix Lengths | /30, /31, /32 and their edge-case behavior |
| Subnetting Formulas | Subnets = 2ⁿ, Usable hosts = 2ⁿ − 2 |
| How to Identify a Subnet | Binary conversion + zeroing host bits, with two worked examples |
| FLSM vs. VLSM | Fixed-size vs. variable-size subnetting, with a worked VLSM table |

---

## Quick-Reference: Prefix Cheat Sheet

| Prefix | Mask | Host Bits | Usable Hosts | Typical Use |
|---|---|---|---|---|
| /24 | 255.255.255.0 | 8 | 254 | Full standard LAN |
| /25 | 255.255.255.128 | 7 | 126 | Larger department |
| /26 | 255.255.255.192 | 6 | 62 | Mid-size department |
| /27 | 255.255.255.224 | 5 | 30 | Small team / guest net |
| /28 | 255.255.255.240 | 4 | 14 | Small office / closet switch |
| /29 | 255.255.255.248 | 3 | 6 | Tiny subnet |
| /30 | 255.255.255.252 | 2 | 2 | Point-to-point link |
| /31 | 255.255.255.254 | 1 | 2 (special case, RFC 3021) | Point-to-point link |
| /32 | 255.255.255.255 | 0 | 1 (host route only) | Static route to a single host |

---

## Suggested Reading Order

1. **CIDR basics** — get comfortable with octets and how prefix length maps to network vs. host bits.
2. **Special prefixes** — /30–/32 are exceptions to the "2ⁿ − 2" rule, so learn them as special cases, not extensions of the formula.
3. **The two core formulas** — memorize these; everything else is applying them.
4. **Identifying a subnet** — practice the binary zero-out method until it's automatic. Finish Example 2 in the notes yourself before checking your work (it's left as a worked exercise).
5. **FLSM vs. VLSM** — this is where the formulas turn into actual network *design*. The scenario below is built to make this section click.

---

## Why This Actually Matters: A Real-World Scenario

### The setup

**Acme Co.** just leased office space and its ISP hands over a single block: **192.168.10.0/24** — 256 addresses, 254 usable. IT needs to fit the entire office network inside that one block:

| Segment | Hosts Needed | Why |
|---|---|---|
| HQ office LAN | 100 | Employee laptops/desktops |
| Sales & Marketing | 40 | Separate department switch |
| Guest Wi-Fi | 20 | Visitor devices |
| Server / data closet | 10 | Internal servers |
| WAN link to Branch A | 2 | Point-to-point router link |
| WAN link to Branch B | 2 | Point-to-point router link |

### What happens if you *don't* subnet

If IT just plugs everything into the flat /24 with no subdivision, three problems show up almost immediately:

- **One giant broadcast domain.** Every ARP request, DHCP broadcast, and misbehaving device's traffic reaches all 254 hosts — including guest Wi-Fi laptops sitting next to production servers.
- **No security boundary.** A guest on the visitor Wi-Fi is on the same logical network as the file server. Isolating them later means re-cabling and re-addressing a live network.
- **No room to reason about growth.** Point-to-point router links (which only ever need 2 addresses) would eat a spot in the same flat pool as a 100-user LAN, with no structure to tell them apart.

### Designing the fix with VLSM

Following the method from the notes — **list every subnet, sort largest-to-smallest, allocate the biggest first, then move the pointer forward** — the plan looks like this:

| Segment | Hosts Needed | Prefix (2ⁿ−2 ≥ needed) | Usable Hosts | Address Range |
|---|---|---|---|---|
| HQ LAN | 100 | /25 | 126 | 192.168.10.0 – 192.168.10.127 |
| Sales & Marketing | 40 | /26 | 62 | 192.168.10.128 – 192.168.10.191 |
| Guest Wi-Fi | 20 | /27 | 30 | 192.168.10.192 – 192.168.10.223 |
| Server closet | 10 | /28 | 14 | 192.168.10.224 – 192.168.10.239 |
| WAN link – Branch A | 2 | /30 | 2 | 192.168.10.240 – 192.168.10.243 |
| WAN link – Branch B | 2 | /30 | 2 | 192.168.10.244 – 192.168.10.247 |
| *(unused, reserved)* | — | /29 | 6 | 192.168.10.248 – 192.168.10.255 |

Every requirement fits inside the single /24 Acme was given, with **8 addresses left over** for whatever comes next (a small IoT VLAN, a third branch link, etc.) — no call to the ISP for more space needed.

### Why FLSM couldn't do this

Try solving the same requirement with FLSM (one fixed borrow size for every subnet). Acme needs **6 subnets**, so it must borrow enough bits to create at least 6 equal ones:

- 2ⁿ ≥ 6 → n = 3 borrowed bits → 8 equal subnets, each a **/27 with 30 usable hosts**.

That single subnet size has to serve *every* segment — including the 100-host HQ LAN and the 40-host Sales department. Both blow past the 30-host ceiling. FLSM simply cannot satisfy this requirement out of one /24; Acme would be forced to request additional address space just because the subnet sizes weren't tailored to actual need — exactly the waste-vs-flexibility trade-off the notes describe, made concrete.

### The payoff

- **Broadcast domains are contained** — a chatty device in Sales doesn't flood the server closet.
- **Guest Wi-Fi is genuinely isolated** — a firewall rule at the subnet boundary (192.168.10.192/27) can block guest traffic from ever reaching 192.168.10.224/28 (servers), something impossible on a flat network.
- **WAN links are cheap** — 2 addresses each via /30, instead of wasting a much larger block on a link that will only ever have two interfaces.
- **The whole design fits in the address space Acme was actually given**, with headroom to grow.

---

## Try It Yourself

1. Finish **Example 2** from the source notes (192.168.29.219/29) using the same zero-out method as Example 1.
2. Extend the Acme scenario: IT just added a **third branch office link** (2 hosts) and a **12-host IoT/security-camera VLAN**. Using the 8 leftover addresses (192.168.10.248/29), figure out whether both new segments fit — and if not, what you'd change.
