# Lab 09: ACLs

## Topology
```
SALES 192.168.10.0/24 ─ gi0/1 [R1] gi0/0 ─ SERVERS 10.0.0.0/24 (Web 10.0.0.10, FTP 10.0.0.20)
HR    192.168.20.0/24 ─ gi0/2
```

## Rules to build
1. **SALES** can only reach the web server on HTTP/HTTPS. Everything else to the servers is blocked.
2. **HR** can reach all servers, but **not** the SALES network.
3. Only **192.168.20.50** (the admin PC) can SSH to R1.

<details>
<summary>✅ Solution</summary>

```
! 1: extended ACL, placed close to the source (SALES, inbound)
ip access-list extended SALES_IN
 permit tcp 192.168.10.0 0.0.0.255 host 10.0.0.10 eq 80
 permit tcp 192.168.10.0 0.0.0.255 host 10.0.0.10 eq 443
 deny   ip 192.168.10.0 0.0.0.255 10.0.0.0 0.0.0.255
 permit ip any any
interface gi0/1
 ip access-group SALES_IN in

! 2: standard ACL, placed close to the destination (SALES LAN, outbound)
ip access-list standard NO_HR_TO_SALES
 deny   192.168.20.0 0.0.0.255
 permit any
interface gi0/1
 ip access-group NO_HR_TO_SALES out

! 3: restrict VTY
ip access-list standard SSH_ADMIN
 permit host 192.168.20.50
line vty 0 4
 access-class SSH_ADMIN in
```
</details>

## Verify
`show access-lists` (do the hit counters go up?) · `show ip interface gi0/1 | include access list`

## 🔨 Break it
Remove the `permit ip any any` from SALES_IN. What else stops working, and why? (Hint: the implicit deny.)
