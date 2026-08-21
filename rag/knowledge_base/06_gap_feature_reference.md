# Gap Feature Reference — Features NOT Supported in Cloud Configuration

**Source:** [S1] https://documentation.meraki.com/Switching/Cloud_Management_with_IOS_XE

These features have **no native Dashboard equivalent** in Cloud Configuration mode.
When detected in a switch configuration, flag them prominently and recommend:
- **Device Configuration mode** if features are operationally critical.
- OR document as residual CLI with a Jinja2 template for Device Config redeployment.

## Gap Feature Catalogue

| Feature | Category | Cloud Config | Device Config | Jinja2 Template | Notes |
|---------|----------|-------------|---------------|-----------------|-------|
| NetFlow / IPFIX | Telemetry | ❌ NOT SUPPORTED | ✅ CLI only | `netflow.j2` | No Dashboard equivalent [S1] |
| EEM Applets | Automation | ❌ NOT SUPPORTED | ✅ CLI only | `eem.j2` | No Dashboard equivalent [S1] |
| Guest Shell | Automation | ❌ NOT SUPPORTED | ✅ CLI only | — | |
| MPLS LDP | Transport | ❌ NOT SUPPORTED | ✅ CLI only | `mpls.j2` | [S1] |
| Segment Routing / SRv6 | Transport | ❌ NOT SUPPORTED | ✅ CLI only | — | |
| TrustSec / SGT Policy | Security | ❌ NOT SUPPORTED | ✅ CLI only | `trustsec.j2` | [S1] |
| Complex MACsec (CKN/CAK) | Security | ❌ NOT SUPPORTED | ✅ CLI only | — | Basic MACsec: PARTIAL [S5] |
| PTP Boundary / Transparent | Timing | ❌ NOT SUPPORTED | ✅ CLI only | `ptp.j2` | [S1] |
| Non-EVPN VXLAN | Overlay | ❌ NOT SUPPORTED | ✅ CLI only | `vxlan.j2` | EVPN-VXLAN: NATIVE at 17.18.2 [S5] |
| IS-IS | Routing | ❌ NOT SUPPORTED | ✅ CLI only | — | |
| MSDP / Anycast-RP | Multicast | ❌ NOT SUPPORTED | ✅ CLI only | — | |
| Router-grade NAT | Routing | ❌ NOT SUPPORTED | ✅ CLI only | — | [S1] |
| Policy-Based Routing (PfR) | Routing | ❌ NOT SUPPORTED | ✅ CLI only | — | [S1] |
| GETVPN | VPN | ❌ NOT SUPPORTED | ✅ CLI only | — | |
| DMVPN | VPN | ❌ NOT SUPPORTED | ✅ CLI only | — | |
| FlexVPN | VPN | ❌ NOT SUPPORTED | ✅ CLI only | — | |
| Non-default VRFs (mgmt path) | L3 | ❌ NOT SUPPORTED | ⚠️ Global VRF only | — | [S3] |
| SNMP (custom traps/OIDs) | Management | 🔁 REDUNDANT | ✅ CLI only | — | Dashboard provides built-in SNMP-like monitoring |
| TACACS+ / RADIUS admin auth | AAA | 🔁 REDUNDANT | ✅ CLI only | — | Dashboard handles device admin auth |
| VTP (VLAN Trunking Protocol) | L2 | 🔁 REDUNDANT | ✅ CLI only | — | Meraki uses flat VLAN model |
| Syslog (local / remote) | Logging | 🔁 REDUNDANT | ✅ CLI only | — | Dashboard event log supersedes |

## Jinja2 Templates Available

Templates are located in:
`ansible/meraki-onboard/roles/deploy_features_cli/templates/`

| Template | Gap Feature |
|----------|------------|
| `netflow.j2` | NetFlow v9 exporter |
| `eem.j2` | EEM applet (syslog event) |
| `trustsec.j2` | TrustSec / SGT manual policy |
| `mpls.j2` | MPLS LDP |
| `ptp.j2` | PTP boundary clock |
| `vxlan.j2` | Non-EVPN VXLAN NVE |

All templates use `NOT NATIVELY SUPPORTED IN CLOUD CONFIG` header and reference [S1]/[S3].
