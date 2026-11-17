# 🩺 Common Scenarios

### 1. PC can ping its gateway, but not the Internet
- Check: `show ip route` on the router. Is there a default route?
- Check: NAT. `show ip nat translations` is empty → wrong inside/outside, or the ACL doesn't match the LAN.
- Check: DNS. Can you ping `8.8.8.8` but not `google.com`? Then it's a DNS problem.

### 2. Two PCs in the same VLAN on different switches can't talk
- `show interfaces trunk`: is the trunk up? Is the VLAN in the **allowed** list and "active"?
- `show vlan brief`: does the VLAN exist on **both** switches?
- Native VLAN mismatch: look for CDP log messages.

### 3. A PC gets a 169.254.x.x address
- APIPA = no DHCP answer. Check `show ip dhcp pool`, and check that the pool isn't full.
- DHCP server in a different subnet? You need `ip helper-address` on the gateway interface.
- DHCP snooping turned on but the uplink isn't **trusted**.

### 4. An OSPF neighbor won't come up
`show ip ospf interface gi0/0` on both sides. Compare: **area, subnet/mask, hello/dead timers, network type, authentication, MTU, passive-interface**. Duplicate router-ids also cause problems.
- Stuck in **EXSTART/EXCHANGE** → MTU mismatch.
- Stuck in **2-WAY** → this is **normal** between two DROTHERs!

### 5. A port suddenly stopped working
`show interfaces status`. Does it say **err-disabled**?
`show port-security interface X` or `show logging` to find the cause, fix it, then `shutdown` / `no shutdown`.

### 6. The network is slow, and the interface shows lots of CRC errors / late collisions
Usually a **duplex mismatch** (one side full, the other half) or a bad cable. Set both sides to auto, or hard-code both sides the same.

### 7. "Everything is slow" + switch CPU is high + lights flashing like crazy
A **broadcast storm / Layer 2 loop**. Check `show spanning-tree`. Did someone turn STP off, or connect two ports together? BPDU Guard prevents this.

### 8. SSH to the router is refused
Check: `ip domain name` + `crypto key` exist, `transport input ssh` is under `line vty`, there's a local user with `login local`, and no VTY `access-class` is blocking you.

### 9. Traffic suddenly blocked after adding an ACL
The **implicit deny** at the end! Did you forget `permit ip any any`? Is it on the right interface **and** direction? Check the hit counters with `show access-lists`.
