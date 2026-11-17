# Lab 10: Port Security + DHCP Snooping + DAI

## Topology
```
[R1 DHCP server] ─ gi0/1 [SW1] fa0/1 ─ PC1
                            fa0/2 ─ PC2
                            fa0/3 ─ "Rogue" router running its own DHCP server
```
All the PC ports are in VLAN 10.

## Tasks
1. Port security on fa0/1–2: max 1 MAC, sticky, violation = **restrict**.
2. Turn on DHCP snooping for VLAN 10. Trust **only** gi0/1.
3. Check that PCs get IPs from R1, **not** from the rogue router.
4. Turn on DAI for VLAN 10.

<details>
<summary>✅ Solution</summary>

```
interface range fa0/1 - 2
 switchport mode access
 switchport access vlan 10
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation restrict
ip dhcp snooping
ip dhcp snooping vlan 10
no ip dhcp snooping information option
interface gi0/1
 ip dhcp snooping trust
 ip arp inspection trust
ip arp inspection vlan 10
```
</details>

## Verify
`show port-security` · `show port-security interface fa0/1` · `show ip dhcp snooping binding` · `show ip arp inspection`

## 🔨 Break it
1. Plug a second PC into fa0/1 through a hub. Watch the violation counter.
2. Change the violation mode to `shutdown` and repeat. Then bring the port back up.
