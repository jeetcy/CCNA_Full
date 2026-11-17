# 🧭 My Troubleshooting Method

## 1. Ask questions first
- What exactly doesn't work? For **who**? Since **when**? **What changed**?
- Does it fail every time, or only sometimes?

## 2. Work through the OSI model, bottom-up
| Layer | Check | Commands |
|---|---|---|
| **1 Physical** | Cable, port lights, `up/up`, errors | `show ip int brief`, `show interfaces` (CRC errors, collisions), `show interfaces status` |
| **2 Data Link** | VLAN, trunk, STP, MAC, duplex | `show vlan brief`, `show interfaces trunk`, `show spanning-tree`, `show mac address-table`, `show cdp neighbors` |
| **3 Network** | IP/mask, gateway, routes, ARP | `ping`, `traceroute`, `show ip route`, `show arp`, `ipconfig /all` on the PC |
| **4 Transport** | Is something blocking the port? | `show access-lists`, `telnet <ip> <port>` |
| **7 Application** | DNS, the service itself | `nslookup`, check whether the server process is running |

## 3. Interface status cheat table
| Status / Protocol | What it means | Likely cause |
|---|---|---|
| administratively down / down | Someone shut it | `no shutdown` |
| down / down | Layer 1 problem | Cable unplugged, wrong cable, the other side is shut |
| up / down | Layer 2 problem | Encapsulation mismatch, clocking (serial links), keepalives |
| up / up | ✅ Working | |
| err-disabled | Security feature shut it | Port security, BPDU Guard, EtherChannel misconfig |

## 4. Divide and conquer
`ping` the **gateway** → then the **next hop** → then the **destination**. Wherever the ping fails is where you look.

## 5. Fix → Test → Write it down
Change **one** thing at a time. Test. Then write the problem in [my-log.md](my-log.md).
