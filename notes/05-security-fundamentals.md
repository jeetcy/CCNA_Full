# 5. Security Fundamentals (15%)

## Key terms
- **Vulnerability**: a weakness. **Exploit**: something that uses the weakness. **Threat**: someone or something that might use the exploit. **Mitigation**: what reduces the risk.
- **CIA triad**: Confidentiality, Integrity, Availability.
- Common attacks: DoS/DDoS, spoofing, reflection/amplification, man-in-the-middle, reconnaissance, malware, phishing, password attacks.

## Securing device access
```
R1(config)# enable secret StrongPass1!            ! hashed. Never use "enable password"
R1(config)# service password-encryption           ! weak (type 7) but hides plain-text passwords
R1(config)# security passwords min-length 10
R1(config)# login block-for 120 attempts 3 within 60
R1(config)# banner motd # Authorized access only! #
R1(config)# line console 0
R1(config-line)# exec-timeout 5 0
```

## AAA
- **A**uthentication (who are you?), **A**uthorization (what can you do?), **A**ccounting (what did you do?)

| | TACACS+ | RADIUS |
|---|---|---|
| Owner | Cisco | Open standard |
| Transport | TCP 49 | UDP 1812/1813 |
| Encrypts | The **whole** packet | Only the password |
| Typical use | Admins logging in to devices | Network access (802.1X, VPN, Wi-Fi) |

## ACLs
| Type | Numbers | Matches on | Place it |
|---|---|---|---|
| Standard | 1–99, 1300–1999 | **Source IP only** | **Close to the destination** |
| Extended | 100–199, 2000–2699 | Source, destination, protocol, port | **Close to the source** |

Rules: they are read **top to bottom**, the **first match wins**, there is an **implicit `deny any`** at the end, and you can have **one ACL per interface, per direction, per protocol**.

**Wildcard mask** = 255.255.255.255 minus the subnet mask. Example: /24 → `0.0.0.255`. `host 10.1.1.1` = `10.1.1.1 0.0.0.0`.

```
! Standard named ACL: block 192.168.10.0/24 from reaching the server LAN
R1(config)# ip access-list standard BLOCK_SALES
R1(config-std-nacl)# deny 192.168.10.0 0.0.0.255
R1(config-std-nacl)# permit any
R1(config)# interface gi0/2
R1(config-if)# ip access-group BLOCK_SALES out

! Extended: only allow web traffic from 192.168.20.0/24 to the server 10.0.0.10
R1(config)# ip access-list extended WEB_ONLY
R1(config-ext-nacl)# permit tcp 192.168.20.0 0.0.0.255 host 10.0.0.10 eq 443
R1(config-ext-nacl)# permit tcp 192.168.20.0 0.0.0.255 host 10.0.0.10 eq 80
R1(config-ext-nacl)# deny ip any any log
R1(config)# interface gi0/1
R1(config-if)# ip access-group WEB_ONLY in
```
Checks: `show access-lists` (look at the hit counters!), `show ip interface gi0/1`

## Layer 2 security
**Port Security**
```
SW(config-if)# switchport mode access
SW(config-if)# switchport port-security
SW(config-if)# switchport port-security maximum 2
SW(config-if)# switchport port-security mac-address sticky
SW(config-if)# switchport port-security violation shutdown   ! or restrict / protect
```
| Violation mode | Drops traffic | Log + counter | Shuts the port |
|---|---|---|---|
| protect | ✅ | ❌ | ❌ |
| restrict | ✅ | ✅ | ❌ |
| shutdown (default) | ✅ | ✅ | ✅ (err-disabled) |

To recover an err-disabled port: `shutdown`, then `no shutdown`, or use `errdisable recovery cause psecure-violation`.

**DHCP Snooping**: blocks fake (rogue) DHCP servers. Ports are **untrusted** by default. Trust only the uplink toward the real DHCP server.
```
SW(config)# ip dhcp snooping
SW(config)# ip dhcp snooping vlan 10
SW(config)# no ip dhcp snooping information option   ! often needed when the switch is not a relay
SW(config-if)# ip dhcp snooping trust                 ! on the uplink
```
**Dynamic ARP Inspection (DAI)**: uses the DHCP snooping table to block ARP spoofing.
```
SW(config)# ip arp inspection vlan 10
SW(config-if)# ip arp inspection trust               ! on the uplink
```

## VPNs
- **Site-to-site** (IPsec): connects two offices. Users don't notice it.
- **Remote access** (TLS/SSL or IPsec, e.g. Cisco AnyConnect / Secure Client): one user connects to the office.
- **IPsec** provides: confidentiality (encryption), integrity (hashing), authentication, and anti-replay.

## Wireless security
| | WPA | WPA2 | WPA3 |
|---|---|---|---|
| Encryption | TKIP | **AES-CCMP** | AES-GCMP |
| Personal mode | PSK | PSK | **SAE** (stronger against password guessing) |
| Enterprise mode | 802.1X | 802.1X | 802.1X (192-bit option) |

**802.1X roles**: Supplicant (the client) → Authenticator (switch/AP/WLC) → Authentication Server (RADIUS).
