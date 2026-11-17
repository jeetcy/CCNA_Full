# 1. Network Fundamentals (20%)

## OSI model (remember: "**P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way")

| # | Layer | Data unit | Example | Devices |
|---|---|---|---|---|
| 7 | Application | Data | HTTP, DNS, SSH | |
| 6 | Presentation | Data | TLS, JPEG | |
| 5 | Session | Data | Opening/closing sessions | |
| 4 | Transport | **Segment** (TCP) / Datagram (UDP) | TCP, UDP, port numbers | Firewall |
| 3 | Network | **Packet** | IPv4, IPv6, ICMP, OSPF | **Router**, L3 switch |
| 2 | Data Link | **Frame** | Ethernet, MAC address, 802.1Q | **Switch** |
| 1 | Physical | **Bits** | Cables, signals | Hub, cable |

TCP/IP model: Application (L5-7), Transport (L4), Internet (L3), Link (L1-2).

## TCP vs UDP

| | TCP | UDP |
|---|---|---|
| Connection | Yes: 3-way handshake (SYN, SYN-ACK, ACK) | No |
| Reliable | Yes: ACKs, resends lost data | No |
| Ordering | Sequence numbers | No |
| Speed | Slower | Faster |
| Used for | HTTP/S, SSH, FTP, email | DNS queries, DHCP, VoIP, video, SNMP, TFTP, Syslog |

## Network devices
- **Switch**: forwards frames using **MAC addresses** inside the same LAN.
- **Router**: forwards packets between **different networks** using IP addresses.
- **Firewall**: filters traffic. **Next-Gen Firewall (NGFW)** also inspects applications and has IPS.
- **Access Point (AP)**: connects wireless clients to the wired network.
- **WLC (Wireless LAN Controller)**: centrally manages many APs.

## Topologies
- **2-tier (collapsed core)**: Access + Core/Distribution together. Small/medium campus.
- **3-tier**: Access → Distribution → Core. Large campus.
- **Spine-leaf**: data centers. Every leaf connects to every spine. Leafs never connect to leafs, spines never to spines. (This is what **Cisco ACI** uses!)
- **SOHO**: small office / home office, often one all-in-one router.

## Cables
- **UTP copper**: max **100 m**. Cat5e = 1 Gbps, Cat6a = 10 Gbps.
- **Fiber**: **Multimode** = shorter distance (inside buildings), cheaper. **Single-mode** = long distance (km), more expensive.
- **Straight-through** vs **crossover**: modern devices use **Auto MDI-X**, so it rarely matters now.

## Switching basics
1. Switch receives a frame → **learns the source MAC** and the port it came from.
2. Looks up the **destination MAC**:
   - Known → forward out that one port.
   - Unknown / broadcast (`FFFF.FFFF.FFFF`) → **flood** out all ports in the VLAN except the one it came in on.
3. MAC entries age out after **300 seconds** by default.

## IPv4 addressing

| Class | First octet | Default mask |
|---|---|---|
| A | 1 – 126 | /8 |
| B | 128 – 191 | /16 |
| C | 192 – 223 | /24 |
| D | 224 – 239 | multicast |
| E | 240 – 255 | reserved |

127.x.x.x = loopback.

**Private ranges (RFC 1918)**, which are not routed on the Internet:
- `10.0.0.0/8`
- `172.16.0.0/12` (172.16 – 172.31)
- `192.168.0.0/16`

APIPA: `169.254.0.0/16` (the PC couldn't get an address from DHCP).

👉 Subnetting practice: [../cheatsheets/subnetting.md](../cheatsheets/subnetting.md)

## IPv6
- 128 bits, written in 8 groups of 4 hex digits.
- Shortening rules: drop leading zeros in each group; replace **one** run of all-zero groups with `::`.
  - `2001:0db8:0000:0000:0000:0000:0000:0001` → `2001:db8::1`

| Type | Prefix | Meaning |
|---|---|---|
| Global Unicast | `2000::/3` | Public, like a public IPv4 |
| Unique Local | `FC00::/7` (in practice `FD`) | Private, like RFC 1918 |
| Link-Local | `FE80::/10` | Only works on the local link, created automatically |
| Multicast | `FF00::/8` | One-to-many. (IPv6 has **no broadcast**) |
| Loopback | `::1` | |

**EUI-64**: builds the interface ID from the MAC. Split the MAC in half, insert `FFFE` in the middle, and flip the 7th bit.

## Wireless basics
- **2.4 GHz**: longer range, more interference. Non-overlapping channels are **1, 6, 11**.
- **5 GHz**: faster, shorter range, many more channels.
- **6 GHz**: Wi-Fi 6E / 7.
- **SSID** = the network name. **BSSID** = the AP radio's MAC address.

## Virtualization
- **VM**: a full operating system running on a **hypervisor**. Type 1 = bare metal (ESXi). Type 2 = runs on top of an OS (VirtualBox).
- **Container**: shares the host OS kernel, so it is lighter and faster (Docker).
- **VRF**: splits one router into several separate routing tables.
