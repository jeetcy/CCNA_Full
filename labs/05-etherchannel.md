# Lab 05: EtherChannel (LACP)

## Topology
```
[SW1] gi0/1 ═══ gi0/1 [SW2]
      gi0/2 ═══ gi0/2
```

## Tasks
1. Bundle gi0/1–2 into Port-channel 1 using **LACP**.
2. Make Po1 a trunk.
3. Check that it shows `SU` and both ports show `(P)`.

<details>
<summary>✅ Solution (both switches)</summary>

```
interface range gi0/1 - 2
 channel-group 1 mode active
interface port-channel 1
 switchport mode trunk
```
</details>

## Verify
`show etherchannel summary` · `show interfaces port-channel 1` · `show spanning-tree` (only Po1 should appear, not gi0/1 and gi0/2)

## 🔨 Break it
1. Set SW2 to `mode passive` too (passive + passive). Does it come up?
2. Set gi0/2 on one switch to `speed 100`. What happens to that member port?
