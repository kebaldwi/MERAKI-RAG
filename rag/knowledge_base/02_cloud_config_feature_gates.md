# Cloud Configuration Feature Version Gates

**Source:** [S5] https://documentation.meraki.com/Switching/Cloud_Management_with_IOS_XE/Product_Information/Overviews_and_Datasheets/Cloud_Management_with_IOS_XE_Overview/Cloud_Configuration%3A_Release_Versions_and_Highlights

## Release Version Highlights

| IOS-XE Version | Features Added |
|----------------|---------------|
| **17.15.1** | Base Cloud Config; read-only Cloud CLI; faster boot; MS390 + C9200L + C9300/L/X support |
| **17.18.1** | BGP, VRF, In-Service Software Upgrade (ISSU); full C9200/C9300/C9500H family support |
| **17.18.2** | Cloud EVPN fabric, VRRP, SmartPort Automation |
| **17.18.3** (latest GA) | Uplink Auto-Configuration (UAC); additional cloud fabric enhancements |
| **26.1.1** | C9200CX-8PT-2G Cloud + Device Config support |
| **26.2.1** | C9550 Device Config support |

## Version Gate Rules

When mapping a feature to Dashboard support, apply these gates:

| Feature | Minimum Version | Gate Rule |
|---------|----------------|-----------|
| Base cloud management | 17.15.1 | All Cloud Config eligible models |
| BGP | 17.18.1 | Verify `ios_xe_version >= 17.18.1` |
| VRF | 17.18.1 | Verify `ios_xe_version >= 17.18.1` |
| VRRP | 17.18.1 | Verify `ios_xe_version >= 17.18.1` |
| ISSU | 17.18.1 | Verify `ios_xe_version >= 17.18.1` |
| Cloud EVPN | 17.18.2 | Verify `ios_xe_version >= 17.18.2` |
| SmartPort Automation | 17.18.2 | Verify `ios_xe_version >= 17.18.2` |
| Uplink Auto-Config (UAC) | 17.18.3 | Verify `ios_xe_version >= 17.18.3` |

## Always NATIVE (any Cloud Config version)

- VLANs, VLAN names
- Access port configuration (VLAN, voice VLAN, PortFast)
- Trunk port configuration (allowed VLANs, native VLAN)
- STP (Rapid-PVST mode, root priority)
- DHCP snooping
- LACP / EtherChannel
- Storm control
- QoS profiles
- ACL basics
- SVI / L3 interface basics
- Port security basics
