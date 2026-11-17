# Lab 02: VLANs & Trunking

## Topology
```
PC1 (VLAN10 192.168.10.11) ─ fa0/1 [SW1] gi0/1 ════ trunk ════ gi0/1 [SW2] fa0/1 ─ PC3 (VLAN10 192.168.10.13)
PC2 (VLAN20 192.168.20.12) ─ fa0/2                                    fa0/2 ─ PC4 (VLAN20 192.168.20.14)
```

## Tasks
1. Create VLAN 10 (SALES), 20 (HR), and 99 (NATIVE) on both switches.
2. Put fa0/1 in VLAN 10 and fa0/2 in VLAN 20.
3. Make gi0/1 a trunk with native VLAN 99, allowing VLANs 10, 20, and 99 only, with DTP off.
4. Test: PC1 ↔ PC3 should work ✅. PC1 ↔ PC2 should **not** work ❌ (different VLANs need a router, see Lab 03).

<details>
<summary>✅ Solution (repeat on both switches)</summary>

```
vlan 10
 name SALES
vlan 20
 name HR
vlan 99
 name NATIVE
interface fa0/1
 switchport mode access
 switchport access vlan 10
interface fa0/2
 switchport mode access
 switchport access vlan 20
interface gi0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,99
 switchport nonegotiate
```
</details>

## Verify
`show vlan brief` · `show interfaces trunk` · `show mac address-table`

## 🔨 Break it
1. Change the native VLAN on SW2 only to 1. What does CDP log?
2. Remove VLAN 20 from the allowed list on one side. Which PCs stop working?
