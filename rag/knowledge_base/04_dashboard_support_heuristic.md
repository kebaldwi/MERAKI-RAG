# Dashboard Native-Support Heuristic (Cloud Configuration Mode)

**Sources:** [S1] https://documentation.meraki.com/Switching/Cloud_Management_with_IOS_XE
            [S5] https://documentation.meraki.com/...Cloud_Configuration%3A_Release_Versions_and_Highlights

## Support Level Definitions

| Level | Symbol | Description |
|-------|--------|-------------|
| **NATIVE** | ✅ | Configurable via Meraki Dashboard UI or API. Management is authoritative in the cloud. |
| **PARTIAL** | 🔶 | Supported with fewer options than IOS-XE CLI. Mark "verify in current release." |
| **CLOUD-CLI-ONLY** | 🔵 | Read-only visibility/troubleshooting via Cloud CLI only. Fully configurable only in Device Config mode. |
| **NOT NATIVELY SUPPORTED** | ❌ | No Dashboard equivalent. Must remain in Device Config local CLI or be removed before Cloud Config migration. |
| **REDUNDANT** | 🔁 | Handled at cloud/dashboard layer; local config not replicated and not needed. |

## Feature Classification Table

| Feature | Support Level | Version Gate | Notes |
|---------|--------------|-------------|-------|
| VLANs (add, name, delete) | ✅ NATIVE | 17.15.1 | Full VLAN database management |
| Access port (VLAN, voice VLAN) | ✅ NATIVE | 17.15.1 | |
| Trunk port (allowed VLANs, native) | ✅ NATIVE | 17.15.1 | |
| STP (Rapid-PVST, root priority) | ✅ NATIVE | 17.15.1 | MSTP verify in current release |
| DHCP Snooping | ✅ NATIVE | 17.15.1 | |
| LACP / EtherChannel | ✅ NATIVE | 17.15.1 | |
| Storm Control | ✅ NATIVE | 17.15.1 | |
| QoS profiles | ✅ NATIVE | 17.15.1 | |
| ACL basics | ✅ NATIVE | 17.15.1 | Complex PBR: verify [S5] |
| SVI / L3 interfaces | ✅ NATIVE | 17.15.1 | |
| Port security basics | ✅ NATIVE | 17.15.1 | Max MAC, violation action |
| BGP | ✅ NATIVE | 17.18.1 | Requires ≥ 17.18.1 [S5] |
| VRF | ✅ NATIVE | 17.18.1 | Requires ≥ 17.18.1; Global VRF only for Device Config [S3] |
| VRRP | ✅ NATIVE | 17.18.1 | Requires ≥ 17.18.1 [S5] |
| ISSU | ✅ NATIVE | 17.18.1 | Requires ≥ 17.18.1 [S5] |
| Cloud EVPN fabric | ✅ NATIVE | 17.18.2 | Requires ≥ 17.18.2 [S5] |
| SmartPort Automation | ✅ NATIVE | 17.18.2 | Requires ≥ 17.18.2 [S5] |
| Uplink Auto-Config (UAC) | ✅ NATIVE | 17.18.3 | Requires ≥ 17.18.3 [S5] |
| OSPF (basic) | 🔶 PARTIAL | 17.15.1 | Full OSPF: verify Dashboard version [S5] |
| Dynamic ARP Inspection | 🔶 PARTIAL | 17.15.1 | Verify in current release [S5] |
| MACsec (basic) | 🔶 PARTIAL | verify | Complex MACsec: NOT SUPPORTED |
| Port mirroring / SPAN | 🔵 CLOUD-CLI-ONLY | — | Visible via Cloud CLI; Device Config for full config |
| TACACS+ / RADIUS (admin) | 🔁 REDUNDANT | — | Dashboard handles admin auth; local config not replicated |
| VTP | 🔁 REDUNDANT | — | Meraki uses flat VLAN model; VTP not needed |
| Local AAA / local users | 🔁 REDUNDANT | — | Dashboard manages device access |
| SNMP v2c/v3 | 🔁 REDUNDANT | — | Dashboard provides built-in monitoring |
| NetFlow / IPFIX | ❌ NOT SUPPORTED | — | No Dashboard equivalent; Device Config / CLI-only [S1] |
| MPLS / SR / SRv6 | ❌ NOT SUPPORTED | — | Not supported [S1] |
| GETVPN / DMVPN / FlexVPN | ❌ NOT SUPPORTED | — | Not supported [S1] |
| TrustSec / SGT policy | ❌ NOT SUPPORTED | — | Not supported in Cloud Config [S1] |
| EEM / Guest Shell | ❌ NOT SUPPORTED | — | Not supported [S1] |
| PTP boundary/transparent | ❌ NOT SUPPORTED | — | Not supported [S1] |
| Non-EVPN VXLAN | ❌ NOT SUPPORTED | — | EVPN-VXLAN supported at 17.18.2+ [S5] |
| IS-IS | ❌ NOT SUPPORTED | — | Not supported [S1] |
| MSDP / Anycast-RP | ❌ NOT SUPPORTED | — | Not supported [S1] |
| Non-default VRFs (mgmt path) | ❌ NOT SUPPORTED | — | Global VRF only for Device Config tunnel [S3] |
| Router-grade NAT | ❌ NOT SUPPORTED | — | Not supported [S1] |
| Policy-Based Routing (PfR) | ❌ NOT SUPPORTED | — | Not supported [S1] |

## Gap Feature Impact Summary

If a configuration contains **NOT NATIVELY SUPPORTED** features, recommend:
- **Device Configuration mode** if the features are operationally critical and cannot be removed.
- **Cloud Configuration mode** with residual CLI documentation if the features can be removed or are replaced by Dashboard equivalents.
