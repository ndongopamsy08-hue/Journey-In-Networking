# STP Toolkit: PortFast

CCNA study notes on **PortFast**, one of the Spanning Tree Protocol (STP) toolkit features.

## Overview

PortFast lets a switch port connected to an end host go straight into the STP **forwarding** state, skipping the **listening** and **learning** states. The host can start communicating as soon as it connects, with no waiting.

Cisco expects you to know the other STP toolkit features as well:

- BPDU Guard
- BPDU Filter
- Root Guard
- Loop Guard

## The Problem PortFast Solves

- When an end host connects to a switch port, the port becomes **up/up**, but the host still cannot send or receive data.
- You can see the up/up status with `show ip interface brief`. Even so, any frames the PC sends are discarded by the switch.
- The port is a designated port, but it takes a total of **30 seconds** to reach the forwarding state:
  **15 s listening + 15 s learning**.
- This gives a poor user experience. Users expect the network to work immediately. They usually do not know STP exists; they just notice the internet does not work for 30 seconds after plugging in.
- The wait is unnecessary because there is no risk of a Layer 2 loop between a switch and a PC. Loops occur between switches because switches flood frames; PCs do not.

## How PortFast Solves It

When PortFast is configured on a port, the port enters the forwarding state immediately once it is enabled and connected to a device. It bypasses the listening and learning states.

## Configuring PortFast

There are two ways to configure PortFast.

| Method         | Command                          | Effect                                    |
| -------------- | -------------------------------- | ----------------------------------------- |
| Interface mode | `spanning-tree portfast`         | Enables PortFast only on that one interface |
| Global mode    | `spanning-tree portfast default` | Enables PortFast on all access ports      |

**Interface mode**

```
Switch(config)# interface g0/1
Switch(config-if)# spanning-tree portfast
```

**Global mode**

```
Switch(config)# spanning-tree portfast default
```

The global command enables PortFast on all access ports. In most cases this means all connections to end hosts, but not the connections to other switches. Links between switches are almost always trunk links, because modern LANs use multiple VLANs. Links to end hosts are almost always access links, because most hosts do not need to send and receive traffic in multiple VLANs.

## Important Note

> **NB:** Please do not enable PortFast between switches.

## Quick Summary

- PortFast skips listening (15 s) and learning (15 s) and goes straight to forwarding.
- Use it only on ports connected to end hosts.
- `spanning-tree portfast` (interface) enables it on one port.
- `spanning-tree portfast default` (global) enables it on all access ports.
- Never enable it between switches.
