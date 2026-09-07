# DHCP Notes — README

## What this is
`DHCP_Notes.docx` is a cleaned-up, reorganized version of your original DHCP notes.
The source file mixed bullet notes with a Q&A-style chat conversation (with "PAMSY").
This version keeps all the technical content but restructures it into a single,
logically ordered study document.

## How it's organized
1. **Overview** – what DHCP is, in one sentence.
2. **What DHCP Does** – the plain-language explanation of the problem DHCP solves.
3. **The DORA Process** – Discover, Offer, Request, Acknowledge, explained step by
   step, plus a summary table (message direction + broadcast/unicast).
4. **The Role of the MAC Address** – why the server can reply to the right client
   even though DHCP messages start out as broadcasts.
5. **Static vs Dynamic Addressing** – when to use manual IPs vs DHCP.
6. **DHCP Relay Agents** – why broadcasts can't reach a centralized server, how
   relay agents fix that, and the "one relay agent per subnet" rule, including the
   small-network vs large-network comparison from your Q&A.
7. **Cisco IOS Configuration** – placeholder section. Your original note titled
   this as a topic ("How to configure DHCP in cisco IOS"), but no actual
   configuration steps or commands were present in the note itself, so this
   section is left as a marker for you to fill in (e.g. `ip dhcp pool`,
   `ip dhcp excluded-address`, `ip helper-address`).
8. **Key Takeaways** – a short recap of the core ideas.

## What was changed
- Removed repeated/duplicate explanations (the note explained DORA and relay
  agents multiple times across the Q&A exchange) and merged them into single,
  non-redundant sections.
- Removed conversational filler (e.g. "Would you like me to draw a diagram?",
  "Exactly PAMSY 👌") since it wasn't technical content.
- Fixed minor typos from the original note (e.g. "recived th DHCP clienta" →
  "receive the DHCP client's").
- Added a DORA summary table for quick reference.
- Grouped related ideas under numbered headings so it reads as a document
  rather than a chat log.

## What's still missing
- The **Cisco IOS configuration commands** — the note mentioned this topic but
  never actually contained the commands. You'll want to add that section
  yourself, or share your config notes/commands and I can integrate them.

## Files
- `DHCP_Notes.docx` — the reorganized Word document.
- `README.md` — this file.
