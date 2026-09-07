# VLANs (Virtual Local Area Networks) — CCNA Notes

## Overview
Structured study notes on VLANs, based on personal notes taken while studying on
NetAcad, expanded with additional explanations and Cisco IOS configuration
commands for hands-on practice and revision.

## Contents
- `VLANs_Notes.docx` — the full formatted notes document.

## Topics Covered
1. **LAN & Broadcast Domain Basics** — what a broadcast frame and broadcast domain are.
2. **The Problem VLANs Solve** — why one flat network causes performance and security issues.
3. **What Is a VLAN?** — logical separation at Layer 2, default VLAN 1.
4. **Switch Ports vs Access Ports** — port types and how access ports map to a single VLAN.
5. **Trunk Ports** — carrying multiple VLANs over one link, VLAN tagging, ISL vs. 802.1Q, native VLAN and mismatch issues.
6. **Router on a Stick (ROAS)** — inter-VLAN routing using sub-interfaces.
7. **Cisco IOS Configuration Commands** — ready-to-use commands for:
   - Creating VLANs
   - Configuring access ports (single port and port ranges)
   - Configuring trunk ports (802.1Q)
   - Setting the native VLAN
   - Verifying configuration (`show` commands)
   - Router-on-a-Stick sub-interface configuration

## How to Use
Read through the concepts, then practice the commands in Packet Tracer, GNS3, or
any Cisco IOS lab environment. The verification commands section is useful for
checking your configuration after each step.

## Source
Based on personal NetAcad study notes on VLANs, reorganized and expanded for
clarity and reference.
