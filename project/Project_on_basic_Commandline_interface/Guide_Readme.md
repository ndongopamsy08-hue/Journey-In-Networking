# 🚀 Lab 01 — Cisco IOS CLI Basics

> Hands-on practice for **CCNA beginners** using Cisco Packet Tracer.  
> Read the [study notes](../Notes.md) first, then come back and do this lab.

---

## 🖧 Topology

```
[ PC0 ] ──── [ Switch0 ] ──── [ Router0 ]
```

- 1 Router
- 1 Switch
- 1 PC

> The `.pkt` file already has this topology built — just open it and start typing.

---

## 🎯 What You Will Practice

| # | Task | Command |
|---|---|---|
| 1 | Move to Privileged EXEC mode | `enable` |
| 2 | Move to Global Config mode | `configure terminal` |
| 3 | Set a plain text password *(less secure)* | `enable password` |
| 4 | See the password in plain text | `show running-config` |
| 5 | Replace it with an encrypted password | `enable secret` |
| 6 | Confirm the password is now hidden | `show running-config` |
| 7 | Save your configuration | `copy running-config startup-config` |
| 8 | Use CLI shortcuts | `?`, `Tab`, arrow keys |

---

## 🧑‍💻 Step-by-Step Instructions

**Step 1** — Open `lab-01-cli-basics.pkt` in Cisco Packet Tracer

**Step 2** — Click on **Switch** → go to the **CLI** tab

**Step 3** — Move up through the modes:

```bash
Switch> enable
Switch# configure terminal
```

---

**Step 4** — Set a plain text password first and observe it:

```bash
Switch(config)# enable password HELLO
Switch(config)# exit

Switch# show running-config
```

> 🔍 Look for this line in the output:
> ```
> enable password HELLO
> ```
> You can read the password directly — **this is the problem with `enable password`**.

---

**Step 5** — Now replace it with an encrypted password:

```bash
Switch# configure terminal
Switch(config)# enable secret mysecret
Switch(config)# exit

⚠️ Common Mistakes

- Typing config commands while in `Router>` → you must be in `Router(config)#` first
- Forgetting `copy running-config startup-config` → changes lost after reboot
- Leaving `enable password` without replacing it with `enable secret` → password stays visible

-Privileged EXEC mode → secure with enable secret.

-User EXEC mode (console access) → secure with line console 0 + password + login.
