# 🧮 Subnetting Cheat Sheet

## The magic table (learn this by heart!)
| CIDR | Mask | Block size | Usable hosts |
|---|---|---|---|
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 (point-to-point links) |
| /31 | 255.255.255.254 | 2 | 2 (point-to-point, RFC 3021) |
| /32 | 255.255.255.255 | 1 | 1 (a single host / loopback) |

The same idea works in the 3rd octet: /16 = 255.255.0.0, /20 = 255.255.240.0 (block 16), /22 = 255.255.252.0 (block 4), /23 = 255.255.254.0 (block 2).

**Formulas**
- Usable hosts = 2^(host bits) − 2
- Number of subnets = 2^(borrowed bits)
- Block size = 256 − the mask value in the "interesting" octet

## The 4-step method
Example: **What subnet is 192.168.10.77/27 in?**
1. /27 → mask `255.255.255.224` → the interesting octet is the **4th**.
2. Block size = 256 − 224 = **32** → subnets start at 0, 32, **64**, 96, 128...
3. 77 is between 64 and 95 → **Network = 192.168.10.64**
4. Next subnet = .96, so **Broadcast = 192.168.10.95**, usable range = **.65 – .94** (30 hosts)

Example: **172.16.45.200/20**
1. /20 → `255.255.240.0` → the interesting octet is the **3rd**.
2. Block = 256 − 240 = 16 → 0, 16, 32, **32+16=48**... 45 is in the **32** block.
3. Network = **172.16.32.0**, Broadcast = **172.16.47.255**
4. Usable = 172.16.32.1 – 172.16.47.254 (4094 hosts)

## Practice (answers at the bottom)
1. 10.1.1.130/26: network? broadcast?
2. 192.168.5.14/29: network? usable range?
3. How many usable hosts in a /23?
4. You need 50 hosts. What is the smallest mask?
5. 172.20.100.5/22: network? broadcast?
6. Is 192.168.1.63/26 a usable host address?

<details>
<summary>👀 Show answers</summary>

1. Block 64 → network **10.1.1.128**, broadcast **10.1.1.191**
2. Block 8 → network **192.168.5.8**, usable **.9 – .14** (broadcast .15)
3. 2^9 − 2 = **510**
4. **/26** (62 hosts). A /27 only gives 30.
5. Block 4 in the 3rd octet → network **172.20.100.0**, broadcast **172.20.103.255**
6. **No.** .63 is the broadcast address of 192.168.1.0/26.
</details>

## Free practice websites
- subnettingpractice.com
- subnetipv4.com
