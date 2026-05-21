# Part 2 – VLANs & Layer-2 EtherChannel

**Devices:** DSW-A1 · DSW-A2 · DSW-B1 · DSW-B2 · ASW-A1 · ASW-A2 · ASW-A3 · ASW-B1 · ASW-B2 · ASW-B3

**Objective:** Build the Layer 2 foundation — EtherChannels between distribution switches, trunks from access to distribution, VLAN creation via VTP, access port assignment, and shutdown of unused ports.

---

## VLAN Reference

| VLAN | Name            | Office A | Office B |
|------|-----------------|:--------:|:--------:|
| 10   | PCs             | ✅       | ✅       |
| 20   | Phones          | ✅       | ✅       |
| 30   | Servers         | —        | ✅       |
| 40   | Wi-Fi           | ✅       | —        |
| 99   | Management      | ✅       | ✅       |
| 1000 | Native (unused) | ✅       | ✅       |

---

## Task 1 – EtherChannel: Office A (PAgP)

Protocol: **PAgP** (Cisco proprietary). Mode: `desirable` on both sides.

**DSW-A1 and DSW-A2** — identical config on both:
```
interface range GigabitEthernet1/0/1 - 2
 channel-group 1 mode desirable
interface Port-channel1
 description EtherChannel_to_peer_DSW
```

> `desirable`↔`desirable` works. `auto`↔`auto` never forms — neither side initiates.

---
