# MERAKI-RAG — Cisco Catalyst 9000 / Meraki Cloud Management Onboarding

A **Retrieval-Augmented Generation (RAG)** system and Ansible automation project for
onboarding Cisco Catalyst 9000 IOS-XE switches into **Meraki Cloud Management with
IOS XE** (formerly Cloud Monitoring / Cloud Management).

---

## Overview

This project provides:

1. **System Prompts** — structured prompts for two AI-agent use cases:
   - **Analysis Use Case**: analyse a switch config, decide onboarding eligibility, map features to Dashboard support.
   - **Onboarding Use Case**: generate a complete Ansible project from a feature analysis.

2. **RAG Knowledge Base** — structured Markdown documents indexed for semantic retrieval, covering:
   - Supported models and minimum IOS-XE version matrix
   - Cloud Configuration feature version gates
   - Device Configuration prerequisites
   - Dashboard native-support heuristic
   - Migration caveats and licensing notes
   - Gap feature reference (features NOT supported in Cloud Config)
   - Ansible collections reference

3. **Ansible Project** (`ansible/meraki-onboard/`) — a complete, runnable Ansible project that:
   - Validates onboarding eligibility and prerequisites (preflight)
   - Upgrades IOS-XE firmware via SSH
   - Onboards switches into Meraki Dashboard (Cloud Config or Device Config path)
   - Deploys native features via Meraki API
   - Deploys gap/CLI-only features via IOS-XE CLI (Device Config mode)
   - Provides Jinja2 templates for unsupported features

---

## Repository Structure

```
MERAKI-RAG/
├── README.md
├── prompts/
│   ├── analysis_use_case.md        # System prompt: Analysis Use Case
│   └── onboarding_use_case.md      # System prompt: Onboarding Use Case
├── rag/
│   └── knowledge_base/
│       ├── 00_index.md
│       ├── 01_models_version_matrix.md
│       ├── 02_cloud_config_feature_gates.md
│       ├── 03_device_config_prereqs.md
│       ├── 04_dashboard_support_heuristic.md
│       ├── 05_migration_and_licensing.md
│       ├── 06_gap_feature_reference.md
│       └── 07_ansible_collections_reference.md
└── ansible/
    └── meraki-onboard/
        ├── ansible.cfg
        ├── requirements.yml
        ├── site.yml
        ├── inventory/
        │   └── hosts.yml
        ├── group_vars/
        │   ├── all.yml
        │   └── vault.yml               # encrypt with ansible-vault
        ├── host_vars/
        │   ├── cs-access-01.yml        # example: Cloud Config, C9300-48P
        │   └── cs-core-01.yml          # example: Device Config, C9500-40X
        └── roles/
            ├── preflight/              # eligibility + prereq audit
            ├── upgrade/                # IOS-XE firmware upgrade
            ├── onboard_cloud/          # Cloud Config onboarding (API)
            ├── onboard_device/         # Device Config onboarding (CLI + claim)
            ├── deploy_features_api/    # NATIVE features via cisco.meraki
            └── deploy_features_cli/    # gap/CLI features via cisco.ios
                └── templates/
                    ├── netflow.j2
                    ├── eem.j2
                    ├── trustsec.j2
                    ├── mpls.j2
                    ├── ptp.j2
                    └── vxlan.j2
```

---

## Authoritative Sources

| ID | Resource | URL |
|----|----------|-----|
| S1 | Cloud Management with IOS XE | https://documentation.meraki.com/Switching/Cloud_Management_with_IOS_XE |
| S2 | Cloud Management with IOS XE Overview | https://documentation.meraki.com/Switching/Cloud_Management_with_IOS_XE/Product_Information/Overviews_and_Datasheets/Cloud_Management_with_IOS_XE_Overview |
| S3 | Enable Cloud Management with Device Configuration | https://documentation.meraki.com/Switching/Cloud_Management_with_IOS_XE/Install_and_Get_Started/Enable_Cloud_Management_for_Cisco_Switches_with_Device_Configuration |
| S4 | CLI-managed → Cloud Config conversion | https://documentation.meraki.com/Switching/Cloud_Management_with_IOS_XE/Install_and_Get_Started/Conversion_from_CLI-managed_IOS_XE_Catalyst_Switches_to_Cloud_Management_with_Cloud_Configuration |
| S5 | Cloud Config Release Versions & Highlights | https://documentation.meraki.com/Switching/Cloud_Management_with_IOS_XE/Product_Information/Overviews_and_Datasheets/Cloud_Management_with_IOS_XE_Overview/Cloud_Configuration%3A_Release_Versions_and_Highlights |
| S6 | Cisco Cloud Entitlement for DNA | https://documentation.meraki.com/Platform_Management/Product_Information/Meraki_Licensing/General_Licensing_Information/Cisco_Cloud_Entitlement_for_DNA |
| S7 | Cisco Software Download | https://software.cisco.com/ |

---

## Quick Start — Ansible

### 1. Install Collections

```bash
cd ansible/meraki-onboard
ansible-galaxy collection install -r requirements.yml
```

### 2. Configure Vault

```bash
# Edit the vault stub and encrypt it
ansible-vault edit group_vars/vault.yml
```

Set your real values in `vault.yml`:
```yaml
vault_meraki_api_key:    "YOUR_API_KEY"
vault_meraki_org_id:     "YOUR_ORG_ID"
vault_meraki_network_id: "YOUR_NETWORK_ID"
vault_ansible_user:      "meraki-svc"
vault_ansible_password:  "YOUR_SSH_PASSWORD"
```

Alternatively, set the environment variable:
```bash
export MERAKI_DASHBOARD_API_KEY="your_api_key"
```

### 3. Add Switches

Edit `inventory/hosts.yml` to add your switches.  
Create a `host_vars/<switch-name>.yml` for each switch using the example dictionaries
in `host_vars/cs-access-01.yml` (Cloud Config) or `host_vars/cs-core-01.yml` (Device Config).

### 4. Run Preflight Check (no changes)

```bash
ansible-playbook site.yml --check --tags preflight
```

### 5. Full Onboarding Run

```bash
ansible-playbook site.yml --ask-vault-pass
```

### 6. Per-Tag Selective Runs

```bash
# Firmware upgrade only
ansible-playbook site.yml --tags upgrade --ask-vault-pass

# Onboarding only
ansible-playbook site.yml --tags onboard --ask-vault-pass

# Features via API only
ansible-playbook site.yml --tags features_api --ask-vault-pass

# Gap/CLI features only (Device Config hosts)
ansible-playbook site.yml --tags features_cli --ask-vault-pass
```

---

## Use Cases

### Analysis Use Case

Use the prompt in `prompts/analysis_use_case.md` with any LLM that has access to the
RAG knowledge base.  Paste a Cisco IOS-XE configuration file and the AI will produce:

1. Onboarding eligibility summary
2. Recommended onboarding mode (Cloud vs Device Config)
3. Device Config prerequisite audit
4. Feature → Dashboard support matrix
5. Gap highlights
6. Jinja2 templates for unsupported features
7. Automated onboarding playbooks

### Onboarding Use Case

Use the prompt in `prompts/onboarding_use_case.md` to generate a complete Ansible
project from a feature analysis output.

---

## Supported Models

| Family | Cloud Config | Device Config |
|--------|-------------|---------------|
| MS390 | ✓ 17.15.1+ | ✗ |
| C9200L / C9300 / C9300L / C9300X | ✓ 17.15.1+ | ✓ 17.15.3+ |
| C9200 / C9200CX | ✓ 17.18.1+ | ✓ 17.15.3+ |
| C9500 High-Perf (48Y4C/24Y4C/32C/32QC) | ✓ 17.18.x | ✓ 17.15.3+ |
| C9500 Legacy (12Q/16X/24Q/40X) | ✗ | ✓ 17.15.3+ |
| C9610 / C9350 | ✓ | ✓ 17.18.2+ |
| C9550 | — | ✓ 26.2.1+ |

---

## Reference: Ansible-Galaxy Onboarding Roles

Inspired by: https://gitlab.com/craigerstix/ansible_cat-onboarding

---

## License

MIT
