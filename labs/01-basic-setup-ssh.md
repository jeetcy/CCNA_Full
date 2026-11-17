# Lab 01: Basic Device Setup + SSH

## Topology
```
PC1 (192.168.1.10/24) ── fa0/1 [SW1] gi0/1 ── gi0/0 [R1] (192.168.1.1/24)
                         SW1 VLAN1 mgmt: 192.168.1.2/24
```

## Tasks
1. Set the hostname, enable secret, banner, and console password on R1 and SW1.
2. Give R1 gi0/0 the IP 192.168.1.1/24. Give SW1 the management IP 192.168.1.2 on `interface vlan 1`, with default gateway .1.
3. Turn on SSH v2 with a local user `admin`. Turn off Telnet.
4. From PC1 (Command Prompt), run `ssh -l admin 192.168.1.1`.

<details>
<summary>✅ Solution</summary>

```
! R1
enable
configure terminal
hostname R1
no ip domain-lookup
enable secret Class123!
banner motd # Authorized access only #
service password-encryption
line console 0
 password Cons123!
 login
 logging synchronous
 exec-timeout 10 0
interface gi0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
ip domain name lab.local
crypto key generate rsa modulus 2048
ip ssh version 2
username admin secret Admin123!
line vty 0 4
 login local
 transport input ssh
end
copy running-config startup-config

! SW1 (same base settings, plus:)
interface vlan 1
 ip address 192.168.1.2 255.255.255.0
 no shutdown
ip default-gateway 192.168.1.1
```
</details>

## Verify
`show ip interface brief` · `show ip ssh` · `show users`

## 🔨 Break it
Remove `ip domain name` and try to generate the RSA keys again. What error do you get? Why does SSH need it?
