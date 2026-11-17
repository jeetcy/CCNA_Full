# Lab 07: OSPF Single Area

## Topology (same as Lab 06; remove all the static routes first!)
```
LAN1 10.1.1.0/24 ─ [R1] ─10.0.12.0/30─ [R2] ─10.0.23.0/30─ [R3] ─ LAN3 10.3.3.0/24
                     \_______________10.0.13.0/30_______________/
```
Loopbacks: R1 = 1.1.1.1/32, R2 = 2.2.2.2/32, R3 = 3.3.3.3/32

## Tasks
1. Run OSPF process 1, **area 0**, on all routers, with a manual router-id that matches the loopback.
2. Make the LAN interfaces **passive**.
3. Set the reference bandwidth to 10000 on all routers.
4. Make the R1–R2 and R2–R3 links `point-to-point` (no DR/BDR election).
5. R2 has the Internet: share its default route with `default-information originate`.

<details>
<summary>✅ Solution (R1 shown; repeat the idea on R2/R3)</summary>

```
interface loopback0
 ip address 1.1.1.1 255.255.255.255
router ospf 1
 router-id 1.1.1.1
 auto-cost reference-bandwidth 10000
 passive-interface gi0/2
 network 10.1.1.0 0.0.0.255 area 0
 network 10.0.12.0 0.0.0.3 area 0
 network 10.0.13.0 0.0.0.3 area 0
 network 1.1.1.1 0.0.0.0 area 0
interface gi0/0
 ip ospf network point-to-point

! R2 only
ip route 0.0.0.0 0.0.0.0 203.0.113.1
router ospf 1
 default-information originate
```
</details>

## Verify
`show ip ospf neighbor` (everything **FULL**) · `show ip route ospf` · `show ip ospf interface brief` · `show ip protocols`

## 🔨 Break it (one at a time, and find each using only `show` commands)
1. Change the hello timer on one side: `ip ospf hello-interval 5`
2. Put one side of a link in `area 1`.
3. Give R3 the same router-id as R1.
4. Make R1's link to R2 `passive`.
