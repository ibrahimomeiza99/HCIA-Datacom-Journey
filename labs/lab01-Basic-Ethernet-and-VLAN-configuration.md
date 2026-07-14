# Lab [NUMBER] — [LAB TITLE]

> **Author:** Ibrahim Omeiza
> **Date:** [14/07/2026]
> **Duration:** 100 Minutes
> **Difficulty:**⭐⭐⭐ 
> **Status:** 🔄 In Progress

---

## Objective
This labs demonstrate how to configure VLANs, and PC communication that are in the same VLAN but in diffenrent switch. It also demonstrate
how interfaces are being configured in different modes such as access, trunk and hybrid.


### IP Address Table

| Device | Interface | IP Address | Subnet Mask | Gateway |
|--------|-----------|------------|-------------|---------|
| R1 | GE0/0/0, GE0/0/0 | 192.168.1.1 | 255.255.255.0 | N/A |
| R1 | GE0/0/1 | 192.168.2.1 | 255.255.255.0 | N/A |
| R2 | GE0/0/0 | 192.168.2.2 | 255.255.255.0 | N/A |
| R2 | GE0/0/1 | 192.168.3.1 | 255.255.255.0 | N/A |
| PC1 | ETH | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 |
| PC2 | ETH | 192.168.3.2 | 255.255.255.0 | 192.168.3.1 |

---

## 3. Tools and Environment

| Item | Detail |
|------|--------|
| Simulator / Hardware | eNSP |
| Devices Used | e.g. 2x AR2220 Routers, 1x S5700 Switch |
| Operating System | Windows |
| Huawei VRP Version | [Version if known] |
| Lab Reference | [Textbook, Huawei doc, or course module] |


## 4. Theory and Concepts

> Before diving into commands, summarize the key concept this lab is built on.
> Write it in your own words as if explaining to a junior colleague.

**Key concept:**

**How it works:**

**Why it matters in cybersecurity / real networks:**

---

## 5. Configuration Steps

> Document every command you ran, on which device, and why.
> Be as detailed as possible. Future you will thank present you.

---

### Device: R1

```bash
# Step 1: Enter system view
<Huawei> system-view
[Huawei] sysname R1

# Step 2: Configure interface GE0/0/0
[R1] interface GigabitEthernet0/0/0
[R1-GigabitEthernet0/0/0] ip address 192.168.1.1 255.255.255.0
[R1-GigabitEthernet0/0/0] undo shutdown
[R1-GigabitEthernet0/0/0] quit

# Step 3: Configure static route
[R1] ip route-static 192.168.3.0 255.255.255.0 192.168.2.2
```

**Why:** *(Explain what these commands accomplish)*

---

### Device: R2

```bash
# Step 1: Enter system view
<Huawei> system-view
[Huawei] sysname R2

# Step 2: Configure interface GE0/0/0
[R2] interface GigabitEthernet0/0/0
[R2-GigabitEthernet0/0/0] ip address 192.168.2.2 255.255.255.0
[R2-GigabitEthernet0/0/0] undo shutdown
[R2-GigabitEthernet0/0/0] quit

# Step 3: Configure static route
[R2] ip route-static 192.168.1.0 255.255.255.0 192.168.2.1
```

**Why:** *(Explain what these commands accomplish)*

---

## 6. Verification

> Always verify your configuration. Never assume it worked. Show the actual output.

### Routing Table Check

```bash
[R1] display ip routing-table

# Expected output:
# Route Flags: R - relay, D - download to fib
# Routing Table: Public
# Destination/Mask  Proto  Pre  Cost  NextHop       Interface
# 192.168.3.0/24   Static  60   0    192.168.2.2   GE0/0/1
```

### Interface Status Check

```bash
[R1] display interface brief

# Expected output:
# Interface            PHY    Protocol
# GE0/0/0             up     up
# GE0/0/1             up     up
```

### Connectivity Test (Ping)

```bash
# From PC1 ping PC2
ping 192.168.3.2

# Expected output:
# PING 192.168.3.2: 56 data bytes
# Reply from 192.168.3.2: bytes=56 Sequence=1 ttl=254 time=30 ms
# Reply from 192.168.3.2: bytes=56 Sequence=2 ttl=254 time=20 ms
# Success rate is 100 percent
```

---

## 7. Problems Encountered

> This is the most valuable section. Be honest about every issue you hit.

| # | Problem | Symptom | Root Cause | Fix Applied |
|---|---------|---------|------------|-------------|
| 1 | | | | |
| 2 | | | | |
| 3 | | | | |

### Detailed Problem Writeups

**Problem 1:**
- *What happened:*
- *Error message (if any):*
- *What I tried:*
- *What actually fixed it:*

---

## 8. Key Takeaways

> Summarize what this lab taught you. Write in plain English.

1.
2.
3.

**What I understand now that I didn't before this lab:**

**How this applies to cybersecurity / my role as an analyst:**

---

## 9. Commands Reference Card

> Quick cheat sheet of every important command used in this lab.

| Command | What it does |
|---------|-------------|
| `display ip routing-table` | Shows the routing table |
| `display interface brief` | Shows status of all interfaces |
| `ip route-static [dest] [mask] [nexthop]` | Configures a static route |
| `undo shutdown` | Brings up an interface |
| `ping [IP]` | Tests connectivity |
| `tracert [IP]` | Traces the path to a destination |

---

## 10. References

> Links, books, or documentation you consulted.

- [ ] Huawei HCIA-Datacom Official Curriculum — Chapter [X]
- [ ] [Huawei VRP Command Reference](https://support.huawei.com)
- [ ] [Any YouTube video, blog, or article you used]

---

## 11. Next Steps

> What lab or topic comes next? What do you want to explore further?

- [ ] Try the same topology with dynamic routing (OSPF)
- [ ] Add a loopback interface and advertise it
- [ ] Practice the same commands from memory without notes

---

*Documented by [Your Name] | HCIA-Datacom Learning Journey*
