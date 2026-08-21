# Device Configuration Prerequisites

**Source:** [S3] https://documentation.meraki.com/Switching/Cloud_Management_with_IOS_XE/Install_and_Get_Started/Enable_Cloud_Management_for_Cisco_Switches_with_Device_Configuration

## Prerequisites Checklist

| # | Requirement | IOS-XE Config Evidence | Notes |
|---|-------------|----------------------|-------|
| 1 | **Front-panel uplink port** (NOT mgmt interface) | Interface naming (Gi/Te/Hu in range 1/0/x or 1/1/x) | Management (Gi0/0) is NOT supported |
| 2 | **Global/Default VRF only** | No `vrf definition` on management path | Non-default VRFs on uplink path block tunnel |
| 3 | `ip routing` enabled | `ip routing` present in config | Required for L3 reachability |
| 4 | **Default route** present | `ip route 0.0.0.0 0.0.0.0 <next-hop>` | `ip default-gateway` is NOT supported |
| 5 | `aaa new-model` configured | `aaa new-model` present | Required for local auth |
| 6 | Local login permitted | `aaa authentication login default local` | "local" must be FIRST if TACACS/RADIUS present |
| 7 | Exec authorization for local accounts | `aaa authorization exec default local` | |
| 8 | Onboarding user = **privilege 15** | `username <user> privilege 15` | Must be local, priv-15 |
| 9 | **MFA NOT supported** for onboarding account | No MFA on onboarding user | Use separate priv-15 account if MFA enforced globally |
| 10 | DNS configured | `ip name-server <ip>` + `ip domain lookup` | Required for mutual TLS tunnel |
| 11 | NTP configured | `ntp server <ip/hostname>` | Clock must be correct for TLS certificate validation |
| 12 | `service cloud-mgmt` enabled | `service cloud-mgmt` | Activates the tunnel |

## Smart Switch CLI Differences

For C9350 / C9610 models (Smart Switches), the CLI commands differ:

```
show cloud-mgmt          # (instead of show sdwan)
service cloud-mgmt connect
```

## Remediation Reference

If any prerequisite is missing, the `onboard_device` role applies the fix automatically.
See `roles/onboard_device/tasks/main.yml` for the remediation tasks.
