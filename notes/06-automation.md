# 6. Automation & Programmability (10%)

## Planes
| Plane | Job | Example |
|---|---|---|
| **Data** (forwarding) | Moves user traffic | Forwarding frames and packets, ACLs, NAT |
| **Control** | Decides **how** to forward | OSPF, STP, ARP, MAC learning |
| **Management** | Lets admins manage the device | SSH, SNMP, Syslog |

## Traditional vs controller-based (SDN)
- **Traditional**: you configure each device one by one using the CLI.
- **SDN / controller-based**: a central **controller** manages all devices.
  - **Northbound API** (NBI): apps and scripts talk **to the controller** (usually a REST API).
  - **Southbound API** (SBI): the controller talks **to the devices** (NETCONF, RESTCONF, OpenFlow, OpFlex, SSH).
- Cisco examples: **Catalyst Center** (formerly DNA Center) for campus networks, **ACI / APIC** for data centers, **SD-WAN** for WAN.
- **Underlay** = the physical network. **Overlay** = virtual tunnels on top of it (e.g. VXLAN). **Fabric** = both together.

## REST APIs
Uses HTTP verbs, which match **CRUD**:
| CRUD | HTTP verb |
|---|---|
| Create | POST |
| Read | GET |
| Update | PUT / PATCH |
| Delete | DELETE |

Status codes: **2xx** success (200 OK, 201 Created), **3xx** redirect, **4xx** client error (400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found), **5xx** server error.

REST is **stateless**: every request carries everything it needs, including authentication.

## Data formats
**JSON** (the one you need to know best for CCNA):
```json
{
  "hostname": "R1",
  "interfaces": [
    { "name": "Gi0/0", "ip": "10.0.0.1", "enabled": true },
    { "name": "Gi0/1", "ip": null,       "enabled": false }
  ]
}
```
- `{ }` = **object** (key/value pairs). `[ ]` = **array** (a list).
- Value types: string, number, boolean (`true`/`false`), null, object, array.
- Keys **must** be in "double quotes".

XML uses `<tags>`, and YAML uses indentation (Ansible playbooks are written in YAML).

## Configuration management tools
| | Ansible | Puppet | Chef | Terraform |
|---|---|---|---|---|
| Agent on device? | **Agentless** (uses SSH) | Agent | Agent | Agentless (uses APIs) |
| Model | Push | Pull | Pull | Push |
| Written in | YAML playbooks | Manifests | Recipes (Ruby) | HCL |

## AI / ML in networking (new in CCNA v1.1)
- **Predictive AI**: finds patterns and predicts problems (e.g. "this link will fail soon").
- **Generative AI**: creates new content (e.g. writes configs, explains logs). Always double-check its output!
- **Machine learning**: models that learn from network data (anomaly detection, traffic classification).

## Next step after CCNA
Try this yourself: [CiscoDevNet/pyats-coding-101](https://github.com/CiscoDevNet/pyats-coding-101), and the free Cisco **DevNet Sandbox** (real ACI, Catalyst Center and IOS-XE you can call with APIs).
