# Lab 08: DHCP + NAT/PAT

## Topology
```
PCs (DHCP) ─ [SW1] ─ gi0/1 [R1] gi0/0 ─ 203.0.113.0/30 ─ [ISP] ─ Server 8.8.8.8
                   192.168.1.1/24       .2            .1
```

## Tasks
1. Make R1 the DHCP server for 192.168.1.0/24. Exclude .1–.10. Gateway .1, DNS 8.8.8.8.
2. Set the PCs to DHCP and check that each one gets an IP address.
3. Set up **PAT** (overload) so all the PCs share R1's gi0/0 address.
4. Give R1 a default route to the ISP.
5. Bonus: move the DHCP server to another router and use `ip helper-address` on R1.

<details>
<summary>✅ Solution</summary>

```
ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp pool LAN
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8
interface gi0/1
 ip address 192.168.1.1 255.255.255.0
 ip nat inside
 no shutdown
interface gi0/0
 ip address 203.0.113.2 255.255.255.252
 ip nat outside
 no shutdown
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 interface gi0/0 overload
ip route 0.0.0.0 0.0.0.0 203.0.113.1
```
</details>

## Verify
`show ip dhcp binding` · `show ip nat translations` (look at the different port numbers!) · `ipconfig /all` on a PC

## 🔨 Break it
1. Swap `ip nat inside` and `outside`. What breaks?
2. Change the ACL to 192.168.2.0. The PCs can ping R1 but not 8.8.8.8. Why?
