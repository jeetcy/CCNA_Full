# 📝 Practice Quiz (30 questions)

Try to answer before you click "Answer". Score yourself: **25+ = ready to move on** 💪

## Network Fundamentals
**1.** At which OSI layer does a router work, and what is the data unit called?
<details><summary>Answer</summary>Layer 3 (Network). The unit is a <b>packet</b>.</details>

**2.** Name the three private IPv4 ranges.
<details><summary>Answer</summary>10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16</details>

**3.** What does a switch do with a frame whose destination MAC isn't in its table?
<details><summary>Answer</summary>It <b>floods</b> the frame out all ports in that VLAN, except the port it came in on.</details>

**4.** Shorten `2001:0db8:0000:0000:0001:0000:0000:0001`.
<details><summary>Answer</summary><code>2001:db8::1:0:0:1</code>. Only <b>one</b> <code>::</code> is allowed, so use it for the first (or longest) run of zeros.</details>

**5.** Which IPv6 address type starts with FE80::/10?
<details><summary>Answer</summary>Link-local</details>

**6.** Which three 2.4 GHz channels don't overlap?
<details><summary>Answer</summary>1, 6, 11</details>

**7.** What's the usable host range of 192.168.1.100/28?
<details><summary>Answer</summary>Network .96, broadcast .111, usable <b>.97 – .110</b></details>

## Network Access
**8.** What happens when the native VLAN doesn't match on the two ends of a trunk?
<details><summary>Answer</summary>Untagged traffic leaks between the two VLANs. CDP logs a "native VLAN mismatch" error.</details>

**9.** Which LACP mode pairs will form an EtherChannel?
<details><summary>Answer</summary>active–active and active–passive. (passive–passive does NOT.)</details>

**10.** How is the STP root bridge chosen?
<details><summary>Answer</summary>The lowest Bridge ID: priority first, then the lowest MAC address.</details>

**11.** What is the STP cost of a 1 Gbps link?
<details><summary>Answer</summary>4</details>

**12.** What does BPDU Guard do?
<details><summary>Answer</summary>If a BPDU arrives on that port, the port is put in <b>err-disabled</b> state. Use it on PortFast ports.</details>

**13.** Which protocol and UDP ports do lightweight APs use to talk to the WLC?
<details><summary>Answer</summary>CAPWAP: UDP 5246 (control) and 5247 (data).</details>

## IP Connectivity
**14.** A router has routes to 10.0.0.0/8 (OSPF), 10.1.0.0/16 (static), and 10.1.1.0/24 (RIP). Which one is used for 10.1.1.5?
<details><summary>Answer</summary>The <b>/24 (RIP)</b>. The longest prefix match always wins first. AD only matters for prefixes of the same length.</details>

**15.** What's the AD of OSPF? Of a static route? Of eBGP?
<details><summary>Answer</summary>110, 1, 20</details>

**16.** What is a floating static route?
<details><summary>Answer</summary>A backup static route with a higher AD than the main route. It's only installed if the main route disappears.</details>

**17.** Two OSPF routers are stuck in EXSTART. What's the most likely cause?
<details><summary>Answer</summary>An MTU mismatch.</details>

**18.** How is the OSPF Router ID chosen?
<details><summary>Answer</summary>1) the manual <code>router-id</code>, 2) the highest loopback IP, 3) the highest active physical interface IP.</details>

**19.** What's the default HSRP priority, and which router becomes Active?
<details><summary>Answer</summary>100. The highest priority wins (if tied, the highest IP). Preemption is off by default.</details>

## IP Services
**20.** Put the four DHCP messages in order.
<details><summary>Answer</summary>Discover, Offer, Request, Acknowledge (DORA)</details>

**21.** Which command lets a router forward DHCP requests to a server in another subnet?
<details><summary>Answer</summary><code>ip helper-address &lt;server-ip&gt;</code> on the interface facing the clients.</details>

**22.** Which keyword turns NAT into PAT?
<details><summary>Answer</summary><code>overload</code></details>

**23.** Syslog level 4 is called...?
<details><summary>Answer</summary>Warning</details>

**24.** Which SNMP version supports encryption?
<details><summary>Answer</summary>SNMPv3</details>

## Security
**25.** Where should you place a standard ACL? An extended ACL?
<details><summary>Answer</summary>Standard: close to the <b>destination</b>. Extended: close to the <b>source</b>.</details>

**26.** What's the wildcard mask for a /26?
<details><summary>Answer</summary>0.0.0.63</details>

**27.** Which port-security violation mode drops traffic and logs it, but keeps the port up?
<details><summary>Answer</summary>restrict</details>

**28.** TACACS+ vs RADIUS: which one encrypts the whole packet?
<details><summary>Answer</summary>TACACS+ (TCP 49). RADIUS only encrypts the password.</details>

## Automation
**29.** Which HTTP verb matches "Read"? Which matches "Create"?
<details><summary>Answer</summary>GET = Read, POST = Create</details>

**30.** Is Ansible agent-based or agentless? Push or pull?
<details><summary>Answer</summary>Agentless (uses SSH), push model.</details>
