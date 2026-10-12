# DHCP Notes — README

## What this is
`DHCP_Notes.docx` is a cleaned-up, fully reorganized version of my DHCP study notes for CCNA. The source material mixed bullet notes with a Q&A-style chat conversation. This version merges everything — including the Cisco IOS configuration commands, which were originally a separate chat log — into a single, logically ordered study document.

## How it's organized
1. **Overview** – what DHCP is, in one sentence.
2. **What DHCP Does** – the plain-language explanation of the problem DHCP solves, plus where it's used and who typically runs the server.
3. **The DORA Process** – Discover, Offer, Request, Acknowledge, explained step by step, plus a summary table (message direction + broadcast/unicast).
4. **The Role of the MAC Address** – why the server can reply to the right client even though DHCP messages start out as broadcasts.
5. **Static vs Dynamic Addressing** – when to use manual IPs vs DHCP.
6. **DHCP Relay Agents** – why broadcasts can't reach a centralized server, how relay agents fix that, and the "one relay agent per subnet" rule, including the small-network vs large-network comparison.
7. **Cisco IOS Configuration** – now filled in with real commands:
   - Excluding addresses (`ip dhcp excluded-address`)
   - Creating a pool (`ip dhcp pool`, `network`, `default-router`, `dns-server`, `domain-name`)
   - How the pool decides which addresses to hand out
   - Multiple pools for multiple subnets/VLANs
   - Verifying leases (`show ip dhcp binding`)
   - Configuring a relay agent (route + `ip helper-address`)
   - Making the router itself a DHCP client (`ip address dhcp`)
   - A quick command reference table
8. **Key Takeaways** – a short recap of the core ideas.

## What was changed
- Merged the original bullet notes with the separate Cisco IOS configuration Q&A into one document, instead of leaving section 7 as a placeholder.
- Removed repeated/duplicate explanations (several ideas were explained more than once across the Q&A exchanges) and merged them into single, non-redundant sections.
- Removed conversational filler (follow-up questions, chat asides, emoji reactions) since it wasn't technical content.
- Fixed typos and cleaned up phrasing from the original notes.
- Added summary tables for quick reference: the DORA message table, an IP-range allocation table, and a Cisco command quick-reference table.
- Grouped related ideas under numbered headings so it reads as a study document rather than a chat log.

## Files
- `DHCP_Notes.docx` — the complete, reorganized Word document.
- `README.md` — this file.
