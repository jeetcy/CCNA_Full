# 2. Network Access (20%)

## VLANs
A VLAN = a separate **broadcast domain** on the same switch.

```
Switch(config)# vlan 10
Switch(config-vlan)# name SALES
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
```
- VLAN 1 = default VLAN. Normal range = 1–1005, extended range = 1006–4094.
- Check: `show vlan brief`

## Trunks (802.1Q)
A trunk carries **many VLANs** on one link by adding a **4-byte tag** to each frame.
```
Switch(config-if)# switchport trunk encapsulation dot1q   ! only needed on some older switches
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20,30
Switch(config-if)# switchport trunk native vlan 99
```
- **Native VLAN** = frames sent **untagged**. It must match on both ends, or you get a **native VLAN mismatch** (CDP warns you).
- **DTP** auto-negotiates trunks. Best practice: turn it off with `switchport nonegotiate`.
- Check: `show interfaces trunk`

## CDP and LLDP
| | CDP | LLDP |
|---|---|---|
| Owner | Cisco-only | Open standard (802.1AB) |
| Default on Cisco | On | Off (`lldp run` to enable) |
| Command | `show cdp neighbors [detail]` | `show lldp neighbors [detail]` |

## EtherChannel
Bundles several physical links into **one logical link**, so STP doesn't block them.

| Protocol | Modes | Works when |
|---|---|---|
| **LACP** (open standard) | `active` / `passive` | active+active, active+passive |
| **PAgP** (Cisco) | `desirable` / `auto` | desirable+desirable, desirable+auto |
| Static | `on` | on+on only |

```
Switch(config)# interface range gi0/1 - 2
Switch(config-if-range)# channel-group 1 mode active
Switch(config)# interface port-channel 1
Switch(config-if)# switchport mode trunk
```
All member ports must match: speed, duplex, VLANs, and trunk/access mode.
Check: `show etherchannel summary` (look for **SU** = Layer 2, in use, and **P** = bundled).

## Spanning Tree (STP / Rapid PVST+)
**Why?** It stops **Layer 2 loops** (broadcast storms) when switches have redundant links.

**How the tree is built:**
1. Elect the **Root Bridge**: the lowest **Bridge ID** (priority + MAC). Default priority = 32768 + VLAN ID.
2. Every non-root switch picks **one Root Port**: the lowest cost to the root.
3. Every segment picks **one Designated Port**.
4. Every other port is **Blocked** (Alternate).

| Speed | STP cost |
|---|---|
| 10 Mbps | 100 |
| 100 Mbps | 19 |
| 1 Gbps | 4 |
| 10 Gbps | 2 |

```
Switch(config)# spanning-tree mode rapid-pvst
Switch(config)# spanning-tree vlan 10 root primary        ! or: spanning-tree vlan 10 priority 4096
Switch(config-if)# spanning-tree portfast                 ! only on ports to end devices
Switch(config-if)# spanning-tree bpduguard enable         ! shuts the port if a BPDU arrives
```
Check: `show spanning-tree vlan 10`

**Rapid PVST+ port roles**: Root, Designated, Alternate, Backup.
**Port states**: Discarding, Learning, Forwarding.

## Wireless architectures
- **Autonomous AP**: each AP is configured on its own.
- **Lightweight AP + WLC**: the APs build **CAPWAP** tunnels to the controller.
  - CAPWAP control traffic = UDP **5246**, data traffic = UDP **5247**.
- **Cloud-based**: Meraki.
- **AP modes**: Local (default), FlexConnect, Monitor, Sniffer, Bridge/Mesh, and others.
