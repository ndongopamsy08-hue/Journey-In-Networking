# NAT Notes — Part 2 — README

## What this is
`NAT_Notes_Part2.docx` is a cleaned-up, reorganized version of my Part 2 NAT study notes for CCNA, continuing on from Part 1 (Static NAT). The source material mixed bullet notes with a Q&A-style chat conversation and inline worked examples. This version keeps all the technical content but restructures it into a single, logically ordered study document.

## How it's organized
1. **Overview** – what Part 2 covers: more on static NAT, dynamic NAT, and dynamic PAT.
2. **Static NAT Works in Both Directions** – how a one-to-one mapping also lets an external host initiate contact with an internal host.
3. **Dynamic NAT** – how the router builds mappings automatically, including:
   - The role of the ACL (identifying traffic to translate, not blocking it)
   - The role of the NAT pool
   - A full worked configuration example
   - NAT pool exhaustion
   - What happens when no addresses are available, and how entries time out or get cleared
   - A static-vs-dynamic NAT comparison table
4. **PAT (Port Address Translation)** – why port numbers get translated, a full two-PC worked example showing port reuse and translation, and why PAT is the most widely used form of NAT.
5. **Key Takeaways** – a short recap of the core ideas.

## What was changed
- Removed repeated/duplicate explanations and merged them into single, non-redundant sections.
- Removed conversational filler and typos from the original notes.
- Added a static-vs-dynamic NAT comparison table for quick reference.
- Grouped related ideas under numbered headings so it reads as a study document rather than a chat log.

## Files
- `NAT_Notes_Part2.docx` — the reorganized Word document.
- `README.md` — this file.

## Related
This continues from `NAT_Notes.docx` (Part 1 — Static NAT and the fundamentals).
