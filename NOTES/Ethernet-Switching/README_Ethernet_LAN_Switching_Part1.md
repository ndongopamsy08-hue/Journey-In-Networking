# 🌐 Ethernet Switching — Study Notes

> Layer 2 | Data Link Layer | MAC Addresses | Frame Encapsulation | LAN Switching

---

## 📄 What Is This?

This repository contains study notes on **Ethernet Switching**, covering how data travels through a network at **Layer 2 of the OSI model**. The notes are written clearly with diagrams, analogies, and a self-check quiz to help anyone understand the concepts easily.

---

## 📚 Topics Covered

- **The Data Link Layer** — role, responsibilities, node-to-node delivery
- **What is a LAN?** — local area networks and how routers/switches define boundaries
- **Data Encapsulation** — Data → Segment → Packet → Frame (PDU review)
- **The Ethernet Frame** — Preamble, SFD, MAC addresses, Type/Length field, FCS
- **MAC Addresses** — structure, OUI, burned-in address (BIA)
- **How Switches Learn** — dynamic MAC address table, flooding, known unicast
- **Self-Check Quiz** — test yourself before seeing the answer

---

## 📁 Files

| File | Description |
|------|-------------|
| `Ethernet_Switching_Study_Notes.docx` | Full notes with diagrams, tables, analogies and quiz |
| `README.md` | This file — quick overview of the repository |

---

## 💡 Key Concepts at a Glance

- Two switches connected = **one LAN**
- A router between switches = **two separate LANs**
- Switches learn MAC addresses by reading the **source** of every incoming frame
- Unknown destination → **flood** to all ports
- Known destination → **forward** to the correct port only
- MAC table entries expire after **5 minutes** of inactivity

---

## 🎯 Who Is This For?

Anyone studying **networking fundamentals** — students preparing for exams like **CCNA**, or anyone who wants to understand how Ethernet switching works from the ground up.
