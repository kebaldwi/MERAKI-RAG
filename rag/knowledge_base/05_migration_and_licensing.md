# Migration Caveats and Licensing Notes

## Migration: CLI-managed IOS XE → Cloud Configuration

**Source:** [S4] https://documentation.meraki.com/Switching/Cloud_Management_with_IOS_XE/Install_and_Get_Started/Conversion_from_CLI-managed_IOS_XE_Catalyst_Switches_to_Cloud_Management_with_Cloud_Configuration

### Key Migration Steps (summary — verify against [S4])

1. **Verify eligibility**: model + IOS-XE version ≥ minimum for Cloud Config [S1].
2. **Pre-migration backup**: export full running-config before any changes.
3. **Resolve NOT-SUPPORTED features**: either remove them, document as residual, or choose Device Config mode instead.
4. **Firmware upgrade** (if required): use Cisco Software Download [S7].
5. **Claim device**: via Meraki Dashboard using Cloud ID / serial.
6. **Dashboard takes over**: configuration authority moves to Dashboard; local CLI becomes read-only.

### ⚠️ Downgrade Caveat

> **WARNING:** Reverting from Cloud Configuration back to classic IOS-XE management
> may require a **factory reset**. Confirm in [S4] before proceeding.
> Always take a full configuration backup before migrating.

### Migration Modes Decision Matrix

| Scenario | Recommended Mode | Rationale |
|----------|-----------------|-----------|
| Standard campus L2/L3 switch | Cloud Configuration | Strategic direction; full Dashboard management |
| Switch with NetFlow/EEM/MPLS/TrustSec | Device Configuration | Gap features require local CLI |
| C9500 legacy (12Q/16X/24Q/40X) | Device Configuration | Cloud Config not eligible [S1] |
| MS390 | Cloud Configuration only | Device Config not eligible [S1] |
| Non-default VRFs on uplink path | Device Configuration | Only Global VRF supported by tunnel [S3] |

---

## Licensing

**Source:** [S6] https://documentation.meraki.com/Platform_Management/Product_Information/Meraki_Licensing/General_Licensing_Information/Cisco_Cloud_Entitlement_for_DNA

### Cisco Cloud Entitlement for DNA

- Eligible devices with an existing **Cisco DNA license** may use it for Meraki Cloud Management via the **Cisco Cloud Entitlement for DNA** programme.
- This entitlement is valid until **01 February 2029**.
- After that date, a Meraki licence must be purchased.
- Models covered: see [S6] for the full eligibility list.

### Licensing Checklist

| Item | Action |
|------|--------|
| Does the device have a valid DNA/Smart licence? | Verify in Cisco Smart Software Manager |
| Is the model in the Cloud Entitlement list? | Check [S6] |
| Is the entitlement date still valid? | Valid until 01-Feb-2029 [S6] |
| No DNA licence? | Purchase a Meraki MS licence |
