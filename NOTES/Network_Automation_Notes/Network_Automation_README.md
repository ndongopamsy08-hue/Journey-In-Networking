# Network Automation

CCNA study notes: why we automate, the logical planes, SDN, APIs and the southbound and northbound interfaces.

## Table of Contents

1. [Overview](#1-overview)
2. [Why Automate? The Problem with Manual Management](#2-why-automate-the-problem-with-manual-management)
3. [Logical Planes of Network Functions](#3-logical-planes-of-network-functions)
4. [Software-Defined Networking (SDN)](#4-software-defined-networking-sdn)
5. [APIs](#5-apis)
6. [Southbound and Northbound Interfaces](#6-southbound-and-northbound-interfaces)
7. [Quick Summary](#7-quick-summary)

---

## 1. Overview

This note is an introduction to the concepts of network automation. Topics to cover:

- Benefits of network automation
- Logical planes of network functions
- Software-Defined Networking (SDN)
- APIs
- Data serialization (covered in the separate JSON, XML and YAML notes)

## 2. Why Automate? The Problem with Manual Management

Say your company wants to add a **loopback interface** to all of its routers. You connect to R1 using SSH, configure the loopback interface, save the config and exit. Then you do the same for R2, R3, R4, until every router in the organization is configured.

### Downsides of managing a network like this

- **Typos and small mistakes are common.** If you have practiced CCNA labs, you have made typos. Sometimes you notice and fix them immediately; sometimes you only notice later, after a long troubleshooting session that ends with a typo in an IP address.
- **It is time consuming and very inefficient** in large networks.
- Repetitive tasks can be automated and performed in a fraction of the time.
- **It is difficult to make sure every device follows the organization's standard configuration.** Organizations usually have standard configuration templates, and it is important that these policies are upheld everywhere.

> **Standard configuration template:** a blueprint or checklist that tells administrators how devices in the organization should be configured. For example, it may require that all routers and switches have SSH enabled, Telnet disabled, certain VLANs configured, secure passwords, logging enabled and unused ports disabled. This keeps devices consistent and secure instead of every administrator configuring them differently. In a large organization it is hard to check every device manually, so the template helps apply and maintain the same standards across the whole network.

### Benefits of automation

- **Less human error** (fewer typos).
- **More scalable networks.** New deployments, network-wide changes and troubleshooting can be done in a fraction of the time.
- **Network-wide policy can be assured** (standard configurations, software versions, etc.).

> **Network-wide policies:** rules that make sure all devices across the organization are configured and managed consistently. They can include security rules, IP addressing, VLANs, access control, configuration standards and monitoring. In simple terms, the same rules are followed throughout the entire network, which reduces errors and security problems.

## 3. Logical Planes of Network Functions

To understand SDN you first have to understand the logical planes of network functions. Start with two simple questions.

### What does a router do?

- Forwards messages between networks by examining information in the Layer 3 header.
- Uses a routing protocol such as OSPF to share route information with other routers and build a routing table.
- Uses ARP to build an ARP table mapping IP addresses to MAC addresses.
- Uses syslog to keep logs of events that occur.
- Allows a user to connect via SSH and manage it.

### What does a switch do?

- Forwards messages within a LAN by examining information in the Layer 2 header.
- Uses STP to make sure there are no Layer 2 loops in the network.
- Builds a MAC address table by examining the source MAC address of frames, and so on.

These functions can be divided and categorised into logical planes:

| Plane                         | What it does                                                                        | Examples                                                                                  |
| ----------------------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Data plane (forwarding plane) | All tasks involved in forwarding user data traffic from one interface to another    | Router: finds the most specific matching route and forwards out the correct interface. Switch: looks at the destination MAC address and forwards out the correct interface. |
| Control plane                 | Builds the tables and does the other work that tells the data plane how to forward  | OSPF and static routes building the routing table, ARP, STP, MAC address learning         |
| Management plane              | Overhead work to manage the device; does not affect forwarding of user data         | SSH (remote CLI access), syslog                                                           |

### Data plane

A router receives a message, looks for the most specific matching route in its routing table and forwards it out of the appropriate interface to the next hop or the destination. A switch receives a message, looks at the destination MAC address and forwards it out of the appropriate interface.

### Control plane

- How does a device's data plane make its forwarding decisions? Functions that build these tables (and other functions that influence the data plane) are part of the control plane.
- So the control plane controls what the data plane does, for example by building the router's routing table.
- The control plane performs overhead work. OSPF itself does not forward user data packets, but it informs the data plane how packets should be forwarded. Without the control plane, the data plane could not send packets over the network.

> **Remember:** Control plane = **decides** where the packet should go (builds or learns the routing table with OSPF or static routes). Data plane = **forwards** the packet according to that decision.

### Management plane

Like the control plane, the management plane performs overhead work. However, it does not directly affect the forwarding of messages in the data plane. Instead it consists of protocols used to manage devices.

- SSH, used to connect remotely to the CLI over the network.
- Syslog, and so on.

## 4. Software-Defined Networking (SDN)

- SDN is an approach to networking that **centralizes the control plane** into a software application called the **controller**.
- SDN is also called software-defined architecture (SDA) or controller-based networking.
- **Traditional control planes use a distributed architecture.** For example, each router runs OSPF, the routers share routing information, and each calculates its own preferred routes to every destination.
- An SDN controller centralizes control plane functions such as calculating routes. Instead of each switch or router making all the decisions independently, the controller manages and gives instructions to multiple devices.
- There are many different SDN solutions, and the role of the controller and of the network devices differs in each one.
- The devices still have their own data planes, which are responsible for actually forwarding packets.

> **Very important:** The controller does **not** forward every packet itself. The switches and routers still forward the packets. The controller mainly makes and distributes decisions and policies.

## 5. APIs

- An **API** (Application Programming Interface) is a way for one software application to communicate with another application or device.
- The SDN controller can interact **programmatically** with network devices using APIs.
- In SDN, the controller is software and the switches and routers are network devices. The controller uses an API to send instructions to them, for example:

```
Controller -> API -> Switch: "Create VLAN 10."
Controller -> API -> Router: "Use this routing policy."
```

- The important word is **programmatically**: the controller communicates with devices automatically using software and code, instead of an administrator manually logging into every switch and typing commands.

> **Remember:** API = a communication method that allows the SDN controller to talk to and control network devices using software.

## 6. Southbound and Northbound Interfaces

### Southbound Interface (SBI)

- The communication between the controller and the network devices is done through the **southbound interface (SBI)**.
- It is called southbound because in diagrams the controller is usually drawn on top and the network devices at the bottom.
- The term does not refer to a physical interface on a device. It is a software interface that allows the controller and the devices to communicate.
- The SBI consists of a communication protocol and an API.
- Example: if R1 and R2 are managed by a central controller that gives instructions, the controller talks to them with the help of the SBI.
- Using the SBI, the controller communicates with the managed devices and gathers information about them, such as topology, configurations, available interfaces and the number of devices on the network.

### Northbound Interface (NBI)

- The NBI is what lets us interact with the controller: access the data it gathers about the network, program it, and make changes in the network via the SBI.
- It is the way a network administrator or network application communicates with the SDN controller. For example, you could use a network management application to tell the controller *"Create VLAN 10 on these switches"* or *"Show me the status of the network"*. You do not have to configure every switch manually.
- The SDN controller (for example Cisco Catalyst Center) receives your request and decides what needs to be done. It then uses the SBI to tell the actual devices (R1, SW1, etc.) to apply a configuration or policy.

> **Flow:** You → NBI → Controller → SBI → Routers / Switches

### REST API

A REST API is used on the controller as an interface for applications to interact with it. REST is not one specific API; it is a **type** of API.

## 7. Quick Summary

- Manual configuration causes typos, takes time and makes consistent policy hard. Automation reduces errors, scales better and enforces standards.
- Data plane **forwards**, control plane **decides**, management plane **manages** the device.
- SDN centralizes the control plane in a controller. The devices still forward packets with their own data planes.
- API = software-to-software communication. The controller uses APIs to program devices automatically.
- SBI = controller to devices. NBI = administrator or application to controller.
- A REST API is a type of API used on the controller.
