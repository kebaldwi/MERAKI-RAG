# Models & IOS-XE Version Matrix

**Sources:** [S1] https://documentation.meraki.com/Switching/Cloud_Management_with_IOS_XE
            [S3] https://documentation.meraki.com/Switching/Cloud_Management_with_IOS_XE/Install_and_Get_Started/Enable_Cloud_Management_for_Cisco_Switches_with_Device_Configuration

## Two Onboarding Modes

| Mode | Former Name | Config Authority | Cloud CLI |
|------|-------------|-----------------|-----------|
| **Cloud Configuration** | Cloud Management | Dashboard (authoritative) | READ-ONLY |
| **Device Configuration** | Cloud Monitoring | Local CLI (READ+WRITE) | Monitoring/troubleshoot only |

## Supported Models & Minimum IOS-XE Versions

| Model Family | Cloud Config Eligible | Cloud Config Min Version | Device Config Eligible | Device Config Min Version |
|---|---|---|---|---|
| MS390 | ✓ | 17.15.1 | ✗ | Not eligible |
| C9200L | ✓ | 17.15.1 | ✓ | 17.15.3 |
| C9300 (core) | ✓ | 17.15.1 | ✓ | 17.15.3 |
| C9300L | ✓ | 17.15.1 | ✓ | 17.15.3 |
| C9300X | ✓ | 17.15.1 | ✓ | 17.15.3 |
| C9300 (24UB/24H/48H/LM/etc.) | ✓ | 17.18.1 | ✓ | 17.15.3 |
| C9200 | ✓ | 17.18.1 | ✓ | 17.15.3 |
| C9200CX | ✓ | 17.18.1 | ✓ | 17.15.3 |
| C9200CX-8PT-2G | ✓ | 26.1.1 | ✓ | 26.1.1 |
| C9500-48Y4C | ✓ | 17.18.x | ✓ | 17.15.3 |
| C9500-24Y4C | ✓ | 17.18.x | ✓ | 17.15.3 |
| C9500-32C | ✓ | 17.18.x | ✓ | 17.15.3 |
| C9500-32QC | ✓ | 17.18.x | ✓ | 17.15.3 |
| C9500-12Q (legacy) | ✗ | NOT eligible | ✓ | 17.15.3 |
| C9500-16X (legacy) | ✗ | NOT eligible | ✓ | 17.15.3 |
| C9500-24Q (legacy) | ✗ | NOT eligible | ✓ | 17.15.3 |
| C9500-40X (legacy) | ✗ | NOT eligible | ✓ | 17.15.3 |
| C9610 | ✓ | cloud | ✓ | 17.18.2 |
| C9350 | ✓ | cloud | ✓ | 17.18.2 |
| C9550 | — | — | ✓ | 26.2.1 |
| C9300-M (Smart) | ✓ | Claim by Cloud ID | N/A | Fully cloud-managed |

> **NOTE:** "-M" models (e.g., C9300-M) are fully cloud-managed; claim by Cloud ID
> label printed on the device. [S3]

## Eligibility Decision Tree

```
Is the model in the Cloud Config eligible list?
  YES → Is the IOS-XE version >= minimum for Cloud Config?
          YES → Cloud Config eligible (preferred strategic direction)
          NO  → Upgrade required [S7]
  NO  → Is the model in the Device Config eligible list?
          YES → Device Config only (e.g., C9500-legacy)
          NO  → Device NOT supported for any cloud management
```

## Firmware Download

Firmware images are available at: [S7] https://software.cisco.com/
Use the Cisco Software Download portal with your CCO account.
