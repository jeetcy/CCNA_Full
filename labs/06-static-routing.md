# Lab 06: Static & Default Routing

## Topology
```
LAN1 10.1.1.0/24 ─ [R1] ─10.0.12.0/30─ [R2] ─10.0.23.0/30─ [R3] ─ LAN3 10.3.3.0/24
                                         │
                                  "ISP" 203.0.113.0/30 (R2 gi0/2 = .2, ISP = .1)
```
R1 gi0/0 = 10.0.12.1, R2 gi0/0 = 10.0.12.2, R2 gi0/1 = 10.0.23.1, R3 gi0/0 = 10.0.23.2

## Tasks
1. Add a static route on R1 and R3 for each other's LAN.
2. Give R1 and R3 a **default route** to R2.
3. Give R2 static routes to both LANs and a default route to the ISP.
4. Bonus: add a second link from R1 to R3 (10.0.13.0/30) and use a **floating static** route (AD 5) as a backup.

<details>
<summary>✅ Solution</summary>

```
! R1
ip route 0.0.0.0 0.0.0.0 10.0.12.2
! R3
ip route 0.0.0.0 0.0.0.0 10.0.23.1
! R2
ip route 10.1.1.0 255.255.255.0 10.0.12.1
ip route 10.3.3.0 255.255.255.0 10.0.23.2
ip route 0.0.0.0 0.0.0.0 203.0.113.1
! Bonus on R1 (10.0.13.2 = R3 on the new link)
ip route 10.3.3.0 255.255.255.0 10.0.12.2          ! primary  (AD 1)
ip route 10.3.3.0 255.255.255.0 10.0.13.2 5        ! floating (AD 5), only used if the primary disappears
```
Only the AD 1 route shows in `show ip route`. The floating route waits in the background.
</details>

## Verify
`show ip route` · `show ip route static` · `traceroute 10.3.3.10`

## 🔨 Break it
1. Shut down **R1 gi0/0**. Does the floating route appear? (`show ip route 10.3.3.0`)
2. Now bring it back and shut **R2 gi0/1** instead. Does R1 switch to the backup? (Hint: **no!** R1 can still reach 10.0.12.2, so it keeps the primary route and the traffic is lost. This is why dynamic routing like OSPF is better.)
