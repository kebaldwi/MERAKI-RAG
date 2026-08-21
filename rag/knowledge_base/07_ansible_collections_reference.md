# Ansible Collections Reference

## Required Collections

| Collection | Purpose | Min Version | Install |
|-----------|---------|-------------|---------|
| `cisco.meraki` | Meraki Dashboard API (onboarding, VLANs, ports, routing, etc.) | ≥ 2.18.0 | `ansible-galaxy collection install cisco.meraki` |
| `cisco.ios` | IOS-XE CLI config / Device Config prereqs | ≥ 6.1.0 | `ansible-galaxy collection install cisco.ios` |
| `ansible.netcommon` | Network connection plugins | ≥ 6.1.0 | `ansible-galaxy collection install ansible.netcommon` |
| `community.general` | Utilities (e.g., `ini_file`, `json_query`) | ≥ 9.0.0 | `ansible-galaxy collection install community.general` |

Install all at once:
```bash
ansible-galaxy collection install -r ansible/meraki-onboard/requirements.yml
```

## Key Modules Used

### cisco.meraki

| Module | Usage in Project |
|--------|----------------|
| `cisco.meraki.meraki_device` | Claim device, set name |
| `cisco.meraki.meraki_vlan` | Create/update VLANs |
| `cisco.meraki.meraki_ms_switchport` | Configure access/trunk ports |
| `cisco.meraki.meraki_ms_link_aggregation` | EtherChannel / LAG |
| `cisco.meraki.meraki_ms_dhcp` | DHCP snooping |
| `cisco.meraki.meraki_ms_stp` | STP configuration |

### cisco.ios

| Module | Usage in Project |
|--------|----------------|
| `cisco.ios.ios_config` | Push IOS-XE configuration blocks |
| `cisco.ios.ios_command` | Run IOS-XE exec commands |
| `cisco.ios.ios_facts` | Gather device facts (post-upgrade verification) |

## Authentication

The `cisco.meraki` collection honours the `MERAKI_DASHBOARD_API_KEY` environment variable:
```bash
export MERAKI_DASHBOARD_API_KEY="your_api_key_here"
```

Alternatively, reference the vault variable:
```yaml
auth_key: "{{ vault_meraki_api_key }}"
```

## Vault Setup

```bash
# Create / encrypt vault
ansible-vault create group_vars/vault.yml

# Edit existing vault
ansible-vault edit group_vars/vault.yml

# Run playbook with vault
ansible-playbook site.yml --ask-vault-pass
# or
ansible-playbook site.yml --vault-password-file ~/.vault_pass
```
