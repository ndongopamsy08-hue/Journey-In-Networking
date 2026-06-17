# 🖧 Cisco IOS CLI — Beginner Notes (Packet Tracer)

> 📘 Study notes for CCNA beginners using Cisco Packet Tracer.  
> Perfect if you're just getting started with network device configuration.

---

## 📖 Table of Contents
- [What is Cisco IOS & CLI?](#what-is-cisco-ios--cli)
- [Execution Modes](#️-execution-modes)
- [Securing Access](#-securing-access-enable-password)
- [Running Config vs Startup Config](#-running-config-vs-startup-config)
- [Helpful CLI Shortcuts](#️-helpful-cli-shortcuts)

---

## 🌐 What is Cisco IOS & CLI?

| Term | Meaning |
|---|---|
| **Cisco IOS** | The operating system that runs on Cisco routers and switches (like Windows, but for network devices) |
| **CLI** | The text interface you use to talk to IOS and configure devices |

---

## ⚙️ Execution Modes

When you open the CLI in Packet Tracer, you always start in **User EXEC mode**.  
You must move up step by step to configure anything.

| Prompt | Mode | What you can do |
|---|---|---|
| `Router>` | User EXEC | Basic monitoring only (`ping`, `show version`...) |
| `Router#` | Privileged EXEC | Full visibility, advanced commands |
| `Router(config)#` | Global Config | Actually configure the device |

**How to move up:**

```bash
Router> enable               # Enter Privileged EXEC mode
Router# configure terminal   # Enter Global Configuration mode
Router(config)#              # You can now configure the device
```

**How to move back down:**

```bash
Router(config)# exit         # Back to Privileged EXEC
Router# disable              # Back to User EXEC
```

---

## 🔑 Securing Access (Enable Password)

To protect Privileged EXEC mode from unauthorized access, set a password:

```bash
# ❌ Less secure — stored in plain text
Router(config)# enable password mypassword

# ✅ Recommended — stored encrypted
Router(config)# enable secret mysecret
```
 **Note:** If both are configured, `enable secret` always overrides `enable password`.

---
 
