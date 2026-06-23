# 🖥️ LAN Topology & ARP/MAC Learning Project

## 📌 Overview
This project demonstrates how a simple **LAN topology** works when devices communicate for the first time.  
It focuses on **MAC address learning** in switches and **ARP table population** in PCs using Packet Tracer.

The topology includes:
- 4 PCs (PC1, PC2, PC3, PC4)
- 2 Switches (SW1, SW2)
- IP addressing scheme: `192.168.1.0/24`

PC1 and PC2 connect to **SW1**, PC3 and PC4 connect to **SW2**, and the two switches are linked together.

---

## 🎯 Project Goals
1. Observe how **ARP requests and replies** are exchanged when one PC pings another for the first time.
2. Understand how switches build their **MAC address tables** dynamically.
3. Practice using **Packet Tracer simulation mode** to visualize message flow.
4. Use **ping** to generate traffic and verify connectivity.
5. Apply **show commands** on switches to identify learned MAC addresses.
6. Learn how to **clear dynamic MAC addresses** from switch tables.

---

## 🛠️ Step-by-Step Procedure
1. **Initial State**  
   - All ARP tables (PCs) and MAC tables (switches) are empty.

2. **Ping from PC1 → PC3**  
   - PC1 sends an **ARP request** asking “Who has 192.168.1.3?”  
   - The request is broadcast → received by SW1, SW2, PC2, and PC4.  
   - PC3 replies with its MAC address → reply goes back to PC1.  
   - Switches learn the source MACs as traffic passes through.

3. **Simulation Mode**  
   - Use Packet Tracer’s simulation to watch ARP and ICMP messages step by step.

4. **Traffic Generation**  
   - Use `ping` between PCs to populate ARP tables and MAC tables.

5. **Verification**  
   - On switches, run:  
     ```
     show mac address-table
     ```  
     to see learned MACs.

6. **Clearing Tables**  
   - On switches, run:  
     ```
     clear mac address-table dynamic
     ```  
     to reset learned entries.

---

## 📚 Learning Outcomes
- Difference between **broadcast (ARP request)** and **unicast (ARP reply)**.
- How switches dynamically learn MAC addresses.
- How ARP tables in PCs are filled during communication.
- Practical use of Packet Tracer commands and simulation mode.

---

## 🔗 Next Steps
- Extend the topology with routers and VLANs.  
- Compare ARP/MAC learning in larger networks.  
- Document results with screenshots and notes in the repo.
