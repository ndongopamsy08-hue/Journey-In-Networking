# NAT Notes — README

## What this is
`NAT_Notes.docx` is a cleaned-up, reorganized version of my NAT study notes for CCNA. The source material mixed bullet notes with a Q&A-style chat conversation and inline analogies. This version keeps all the technical content but restructures it into a single, logically ordered study document.

## How it's organized
1. **Overview** – what NAT is, in one sentence, and what the document covers.
2. **The IPv4 Address Shortage** – why IPv4 ran out of room, IPv6 as the long-term fix, and the three short-term solutions (CIDR, private addressing, NAT), including the RFC 1918 private address range table.
3. **What NAT Does** – the core definition, why it's used, and how source NAT works at a high level.
4. **Key NAT Terminology** – inside local, inside global, outside local, outside global, in one reference table.
5. **Static NAT** – the one-to-one mapping concept, how the flow works step by step, a post-office analogy, a worked two-PC example, and its main limitation.
6. **Configuring Static NAT** – the configuration steps, an example topology table, the full Cisco IOS command sequence, and verification steps.
7. **Reading the NAT Translation Table** – how static entries display, what the port-number columns mean, and outside local/global.
8. **Managing NAT Translations** – `clear ip nat translations`, `show ip nat static`, and a command reference table.
9. **Key Takeaways** – a short recap of the core ideas.

## What was changed
- Removed repeated/duplicate explanations (several ideas were explained more than once across the original Q&A exchange) and merged them into single, non-redundant sections.
- Removed conversational filler (follow-up questions, chat asides, emoji reactions) since it wasn't technical content.
- Fixed typos and cleaned up phrasing from the original notes.
- Added reference tables: private IPv4 ranges, NAT terminology, the example topology, and a NAT command quick-reference.
- Grouped related ideas under numbered headings so it reads as a study document rather than a chat log.

## Files
- `NAT_Notes.docx` — the reorganized Word document.
- `README.md` — this file.
