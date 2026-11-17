# Lab 04: STP Root Bridge & PortFast

## Topology (a triangle = a loop!)
```
        [SW1]
       /     \
  gi0/1       gi0/2
    /           \
[SW2] ──gi0/2── [SW3]
```
All links are trunks. Add VLAN 10 on all switches, plus one PC on SW2 fa0/1 and one on SW3 fa0/1.

## Tasks
1. Run `show spanning-tree vlan 10` on every switch. Which one is root? Why? (Hint: lowest MAC.)
2. Make **SW1** the root for VLAN 10.
3. Find which port is **blocking** (Alternate). Explain why.
4. Turn on PortFast and BPDU Guard on the PC ports.
5. Use Rapid PVST+.

<details>
<summary>✅ Solution</summary>

```
! all switches
spanning-tree mode rapid-pvst
! SW1
spanning-tree vlan 10 root primary          ! sets priority to 24576 (or lower)
! SW2 + SW3 PC ports
interface fa0/1
 spanning-tree portfast
 spanning-tree bpduguard enable
```
The blocked port ends up on the SW2–SW3 link, on the switch with the **higher Bridge ID**.
</details>

## 🔨 Break it
Plug a new switch into a PortFast + BPDU Guard port. What happens? (`show interfaces status err-disabled`)
