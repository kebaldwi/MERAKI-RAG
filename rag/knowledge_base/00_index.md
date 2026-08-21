# RAG Knowledge Base — Index

This directory contains the RAG (Retrieval-Augmented Generation) knowledge base for the
Meraki Catalyst 9000 Onboarding project.  Each file represents a structured knowledge
chunk that an AI agent can retrieve at runtime to ground its responses.

## Files

| File | Content | Source |
|------|---------|--------|
| `01_models_version_matrix.md` | Supported models and minimum IOS-XE version matrix | [S1] [S3] |
| `02_cloud_config_feature_gates.md` | Cloud Configuration feature version gates | [S5] |
| `03_device_config_prereqs.md` | Device Configuration prerequisites checklist | [S3] |
| `04_dashboard_support_heuristic.md` | Dashboard native-support classification | [S1] [S5] |
| `05_migration_and_licensing.md` | Migration caveats and licensing notes | [S4] [S6] |
| `06_gap_feature_reference.md` | Features NOT supported in Cloud Config | [S1] |
| `07_ansible_collections_reference.md` | Ansible collections and module reference | — |

## Retrieval Strategy

Use semantic chunking on each file.  Each chunk should retain its source citation
(e.g., `[S1]`) so the AI agent can cite the authoritative source in its response.

Index refresh: these documents should be re-fetched from their authoritative URLs
whenever a new Meraki firmware release is announced.
