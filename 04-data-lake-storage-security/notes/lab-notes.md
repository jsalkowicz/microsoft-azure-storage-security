# Lab Notes

## Environment

- Resource group: `rg-adls-security-lab`
- Storage account type: StorageV2
- Hierarchical namespace: enabled
- Data Lake filesystem/container: `enterprise-data`
- Replication: LRS
- Access tier: Hot
- Secure transfer: enabled
- Minimum TLS: 1.2
- Anonymous blob access: disabled
- Storage account key access: disabled in the final state
- Public network access: enabled for the scope of this lab

## Identity Setup

Security groups:

- `sg-adls-claims-analysts`
- `sg-adls-hr-analysts`

Synthetic test users:

- Claims Lab User
- HR Lab User

Both groups received the Azure `Reader` role on the storage account for portal navigation only.

The setup account used `Storage Blob Data Owner` on the lab container while ACLs were being configured.

## Synthetic Data

The CSV files in `data/` were created specifically for this lab.

They contain fake claim IDs, employee names, compensation values, departments, and other made-up business data. They should not be interpreted as real records.

## Scope

This was an identity and access-control lab. It did not attempt to reproduce a full production landing zone.

Public network access remained enabled to keep the focus on Entra authentication, RBAC, and Data Lake ACL behavior. Private endpoints, firewall restrictions, centralized logging, policy enforcement, and enterprise key-management patterns would be separate follow-on work.
