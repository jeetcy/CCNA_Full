# 4. IP Services (10%)

## NAT / PAT
| Term | Meaning |
|---|---|
| Inside local | Private IP of the inside host (e.g. 192.168.1.10) |
| Inside global | Public IP that represents the inside host |
| Outside global | The real public IP of the outside server |

```
! Mark the inside and outside interfaces
R1(config)# interface gi0/1
R1(config-if)# ip nat inside
R1(config)# interface gi0/0
R1(config-if)# ip nat outside

! Static NAT (one-to-one, e.g. for a web server)
R1(config)# ip nat inside source static 192.168.1.10 203.0.113.10

! PAT / NAT overload (many-to-one, what home routers do)
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255
R1(config)# ip nat inside source list 1 interface gi0/0 overload
```
Checks: `show ip nat translations`, `show ip nat statistics`, `clear ip nat translation *`

## DHCP (**DORA**: Discover → Offer → Request → Ack). Server UDP **67**, client UDP **68**
```
R1(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10
R1(config)# ip dhcp pool LAN
R1(dhcp-config)# network 192.168.1.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.1.1
R1(dhcp-config)# dns-server 8.8.8.8
R1(dhcp-config)# lease 7
```
**DHCP relay**: needed when the DHCP server is in a different subnet, because routers don't forward broadcasts:
```
R2(config-if)# ip helper-address 10.0.0.5
```
Checks: `show ip dhcp binding`, `show ip dhcp pool`. On a Windows PC: `ipconfig /release`, then `ipconfig /renew`.

## DNS (UDP/TCP **53**)
Turns names into IPs. Record types: **A** (IPv4), **AAAA** (IPv6), **CNAME** (alias), **MX** (mail), **PTR** (reverse lookup).
```
R1(config)# ip name-server 8.8.8.8
R1(config)# ip domain lookup
```

## NTP (UDP **123**)
Keeps clocks in sync, which matters for logs and certificates. **Stratum** = how far a device is from the reference clock (lower = better).
```
R1(config)# ntp server 192.168.1.100
R1(config)# ntp master 3          ! make this router an NTP server
```
Check: `show ntp status`, `show ntp associations`

## SNMP (UDP **161** for polling, **162** for traps)
- The **Manager** (NMS) asks the **Agent** (device) for information. Data is stored in the **MIB** and identified by **OIDs**.
- Messages: **Get** / **Set** / **Trap** (no acknowledgement) / **Inform** (acknowledged).
- Versions: v1 / v2c (community string, no encryption), **v3 (authentication + encryption) = use this one**.

## Syslog (UDP **514**). Remember the levels: "**E**very **A**wesome **C**isco **E**ngineer **W**ill **N**eed **I**ce cream **D**aily"
| Level | Name |
|---|---|
| 0 | Emergency |
| 1 | Alert |
| 2 | Critical |
| 3 | Error |
| 4 | Warning |
| 5 | Notification |
| 6 | Informational |
| 7 | Debugging |
```
R1(config)# logging 192.168.1.100
R1(config)# logging trap warnings       ! sends levels 0-4
R1(config)# line console 0
R1(config-line)# logging synchronous    ! log messages stop interrupting your typing
```

## QoS basics
- Fixes problems with **delay, jitter, and packet loss**. Voice needs: delay < 150 ms, jitter < 30 ms, loss < 1%.
- Steps: **Classify → Mark** (DSCP **EF = 46** for voice, CoS 5) → **Queue** (LLQ gives voice a priority queue) → **Police** (drop excess) / **Shape** (buffer excess).

## SSH (TCP **22**). Use SSH, never Telnet (TCP 23, sends everything in clear text)
```
R1(config)# hostname R1
R1(config)# ip domain name lab.local
R1(config)# crypto key generate rsa modulus 2048
R1(config)# ip ssh version 2
R1(config)# username admin secret Cisco123!
R1(config)# line vty 0 4
R1(config-line)# login local
R1(config-line)# transport input ssh
```

## File transfer
| | TFTP | FTP |
|---|---|---|
| Transport | UDP 69 | TCP 20 (data) / 21 (control) |
| Login | No | Yes (username/password) |
```
R1# copy running-config tftp:
R1# copy tftp: flash:
```
