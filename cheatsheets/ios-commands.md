# 💻 Cisco IOS Command Cheat Sheet

## Modes
```
Router>                       ! user EXEC
Router> enable
Router#                       ! privileged EXEC
Router# configure terminal
Router(config)#               ! global config
Router(config)# interface gi0/0
Router(config-if)#            ! interface config
Router(config-if)# end        ! straight back to #   (or press Ctrl+Z)
```

## Save / view / erase
| Command | What it does |
|---|---|
| `copy running-config startup-config` (or `wr`) | **Save!** |
| `show running-config` | The config that is active now (in RAM) |
| `show startup-config` | The saved config (in NVRAM) |
| `erase startup-config` + `reload` | Factory reset |
| `show run \| include ospf` | Filter the output |
| `show run \| section interface` | Show only one section |
| `do show ip int brief` | Run a `show` command from config mode |

## Most-used `show` commands
| Command | Use it for |
|---|---|
| `show ip interface brief` | Interface IPs + up/down status (**use this first!**) |
| `show interfaces gi0/0` | Errors, duplex, speed, CRC errors |
| `show interfaces status` | Switch ports: VLAN, duplex, speed |
| `show vlan brief` | VLANs and their access ports |
| `show interfaces trunk` | Trunks and allowed VLANs |
| `show mac address-table` | Which MAC address is on which port |
| `show spanning-tree [vlan X]` | Root bridge, port roles |
| `show etherchannel summary` | Port-channel status |
| `show cdp neighbors detail` | Neighbor devices + their IPs |
| `show ip route` | Routing table |
| `show ip ospf neighbor` | OSPF neighbors |
| `show ip protocols` | Routing protocol details |
| `show access-lists` | ACL rules + hit counts |
| `show ip nat translations` | NAT table |
| `show ip dhcp binding` | DHCP leases |
| `show port-security interface fa0/1` | Port-security status |
| `show standby brief` | HSRP status |
| `show version` | IOS version, uptime, config register |
| `show logging` | Log messages |

## Testing
```
ping 10.1.1.1
ping 10.1.1.1 source gi0/1         ! test from a specific interface
traceroute 10.1.1.1
telnet 10.1.1.1 80                  ! check that a TCP port is open
show arp
```
Ping symbols: `!` success · `.` timeout · `U` unreachable · `M` could not fragment

## Interface basics
```
interface gi0/0
 description LINK-TO-R2
 ip address 10.0.12.1 255.255.255.0
 no shutdown
```

## Handy tricks
- Type `?` anywhere to see help.
- Press **Tab** to auto-complete a command.
- `no ip domain-lookup` stops the router from treating mistyped commands as hostnames.
- `logging synchronous` (under `line con 0`) stops log messages interrupting your typing.
- Press **Ctrl+Shift+6** to stop a ping or traceroute.
