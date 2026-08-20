# OSPF Study Notes

## What this is
`OSPF_Notes.docx` is a cleaned-up and reorganized version of personal study notes on **OSPF (Open Shortest Path First)**, written while preparing for the CCNA exam (topic 3.4 — configuring and verifying single-area OSPFv2).

All of the original content and diagrams have been kept — nothing was removed. The material was only re-arranged into a clear structure with consistent headings, bullet points, and callout boxes so it's easier to study from and reference later.

## Structure of the document

1. **Introduction**
   CCNA exam objectives covered (3.4a–3.4d), what the notes cover, and how OSPF (link-state) differs from distance-vector routing.

2. **Basic OSPF Operation**
   - What OSPF stands for and the Dijkstra algorithm it's based on
   - OSPF versions (v1, v2, v3)
   - Link-state concepts: LSAs, the LSDB, flooding
   - Link and cost
   - The aging timer
   - The three steps of OSPF operation (adjacency, LSA exchange, route calculation)

3. **OSPF Areas**
   - Why large networks are split into areas, and the downsides of using only one area
   - Area 0 (the backbone) and how areas communicate through it
   - Internal routers vs. Area Border Routers (ABRs)
   - Worked example (R2/R3 as ABRs)
   - Intra-area vs. inter-area routes
   - Contiguous vs. non-contiguous area design
   - The "same subnet, same area" rule, with the diagrams showing why a wrongly-placed interface fails to form a neighbor relationship, and how to fix it
   - Key rules highlighted in callout boxes

4. **OSPF Configuration**
   - Entering OSPF configuration mode (`router ospf <process-id>`)
   - What the process ID means
   - Note on CCNA scope (single-area OSPF, typically Area 0)

## Notes for future updates
- This file will be added to alongside a second document (to be provided later) — feel free to keep using this same structure (numbered sections, bullet points, callout boxes for "important" rules) so the notes stay consistent.
- Diagrams are placed directly under the bullet points they illustrate, so keep new images close to their related explanation when adding content.
