# 3. IP Connectivity (25%), the biggest domain!

## Reading the routing table: `show ip route`
```
O    10.1.1.0/24 [110/2] via 192.168.12.2, 00:05:12, GigabitEthernet0/0
^         ^       ^   ^        ^                              ^
code   prefix    AD  metric  next hop                   exit interface
```
Codes: **C** connected, **L** local (/32), **S** static, **O** OSPF, **D** EIGRP, **R** RIP, **B** BGP, `*` = candidate default route.

## How a router picks a route (in this order!)
1. **Longest prefix match** wins first. A /28 beats a /24, which beats a /0.
2. If the prefix lengths are equal, the **lowest Administrative Distance (AD)** wins.
3. If the AD is equal too, the **lowest metric** wins.

| Source | AD |
|---|---|
| Connected | 0 |
| Static | 1 |
| eBGP | 20 |
| EIGRP (internal) | 90 |
| OSPF | 110 |
| IS-IS | 115 |
| RIP | 120 |
| EIGRP (external) | 170 |
| iBGP | 200 |
| Unusable | 255 |

## Static routes
```
R1(config)# ip route 10.2.2.0 255.255.255.0 192.168.12.2        ! next-hop
R1(config)# ip route 10.2.2.0 255.255.255.0 gi0/0               ! exit interface
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1                ! default route
R1(config)# ip route 10.2.2.0 255.255.255.0 192.168.13.3 130    ! floating static (AD 130)
```
**Floating static** = a backup route with a **higher AD** than the main route. It only appears when the main route disappears.

IPv6:
```
R1(config)# ipv6 unicast-routing
R1(config)# ipv6 route 2001:db8:2::/64 2001:db8:12::2
R1(config)# ipv6 route ::/0 2001:db8:12::2
```

## OSPFv2 (single area)
- **Link-state** protocol, AD **110**, metric = **cost** = reference bandwidth / interface bandwidth.
  - Default reference = 100 Mbps, so FastEthernet and GigabitEthernet **both get cost 1**. Fix it with `auto-cost reference-bandwidth 10000`.
- Multicast addresses: **224.0.0.5** (all OSPF routers), **224.0.0.6** (DR/BDR).
- Hello / Dead timers: **10 / 40 s** on broadcast networks.

```
R1(config)# router ospf 1
R1(config-router)# router-id 1.1.1.1
R1(config-router)# network 10.0.12.0 0.0.0.255 area 0
R1(config-router)# passive-interface gi0/1          ! LAN side, no neighbors there
! or configure it on the interface:
R1(config-if)# ip ospf 1 area 0
```

**Router ID order**: manual `router-id` → highest loopback IP → highest physical interface IP.

**Neighbor states**: Down → Init → 2-Way → ExStart → Exchange → Loading → **Full**.

**Neighbors only form if these MATCH**: area ID, subnet/mask, hello/dead timers, authentication, stub flag, and MTU (or they get stuck in ExStart). Router IDs must be **unique**.

**DR/BDR** (on Ethernet/broadcast networks): the **highest priority** wins (default 1, 0 = never). If tied, the **highest Router ID** wins. Elections are **not pre-emptive**.
Point-to-point network type = no DR/BDR election: `ip ospf network point-to-point`.

Checks: `show ip ospf neighbor`, `show ip ospf interface brief`, `show ip protocols`, `show ip route ospf`

## First Hop Redundancy (FHRP)
Gives PCs a **virtual gateway IP** that keeps working if one router fails.

| | HSRP | VRRP | GLBP |
|---|---|---|---|
| Owner | Cisco | Open standard | Cisco |
| Roles | Active / Standby | Master / Backup | AVG / AVF |
| Load balancing | Per group | Per group | Yes, built in |

```
R1(config-if)# standby 1 ip 192.168.1.254
R1(config-if)# standby 1 priority 110      ! default 100, higher wins
R1(config-if)# standby 1 preempt
```
