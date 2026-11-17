# Lab 03: Inter-VLAN Routing (Router-on-a-Stick)

## Topology
Use Lab 02's SW1 and add R1 to SW1 gi0/2 (trunk).
```
[R1] gi0/0 ════ trunk ════ gi0/2 [SW1]
  gi0/0.10 = 192.168.10.1/24   (VLAN 10 gateway)
  gi0/0.20 = 192.168.20.1/24   (VLAN 20 gateway)
```

## Tasks
1. Make SW1 gi0/2 a trunk.
2. Create sub-interfaces on R1 with `encapsulation dot1Q`.
3. Set each PC's default gateway to `.1` in its VLAN.
4. Test: PC1 (VLAN 10) can now ping PC2 (VLAN 20) ✅

<details>
<summary>✅ Solution</summary>

```
! SW1
interface gi0/2
 switchport mode trunk

! R1
interface gi0/0
 no shutdown
interface gi0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
interface gi0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
```
**Bonus: the Layer 3 switch way (SVIs)**
```
ip routing
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown
interface vlan 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown
```
</details>

## Verify
`show ip interface brief` (on R1) · `show ip route` (you should see C and L routes for both VLANs) · `tracert` from a PC

## 🔨 Break it
Change `encapsulation dot1Q 20` to `dot1Q 30`. What happens to VLAN 20 PCs? How would you find the problem?
