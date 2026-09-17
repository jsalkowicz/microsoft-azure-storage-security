# Microsoft Azure Storage Security Labs

**Status:** In Progress — Labs 00–01 complete; Labs 02–03 still in progress
**Platform:** Microsoft Azure  
**Focus:** Azure Storage security, Microsoft Entra ID, Azure RBAC, Shared Key, and Shared Access Signatures

## Project Overview

This repository documents a set of hands-on Azure Storage security labs built around a fictional insurance environment. The exercises use synthetic data only and are designed to demonstrate how storage security controls work in practice.

The work is intentionally structured as a small security-engineering engagement rather than a collection of disconnected tutorials.

## Security Problem

Cloud storage can be encrypted and still be exposed if authentication, authorization, delegated access, or key management is configured poorly.

These labs examine several common questions:

- Who can create and manage Azure resources?
- Which identity can access a specific Blob container?
- How does container-scoped RBAC enforce least privilege?
- What is the risk of using Storage account keys?
- How is key rotation performed without unnecessarily breaking workloads?
- How can temporary access be delegated without sharing a Storage account key?
- How can access be validated and evidenced?

## Architecture

```text
Microsoft Entra ID
       |
       | identities
       v
Azure RBAC
       |
       | scoped permissions
       v
Azure Storage Account
       |
       +---- claims container
       |
       +---- humanresources container
       |
       +---- web-assets container

Additional authorization methods examined:
- Shared Key
- Shared Access Signatures (SAS)
```

## Environment and Technologies

- Microsoft Azure
- Azure Storage / Blob Storage
- Microsoft Entra ID
- Azure Role-Based Access Control (RBAC)
- Azure Cloud Shell / Azure CLI
- Storage account keys
- Shared Access Signatures
- Azure Monitor / Storage diagnostics
- Synthetic insurance data

## Lab Structure

| Lab | Topic | Primary Security Concept |
|---|---|---|
| 00 | Account, Permissions & RBAC Setup | **Complete** — container-scoped RBAC validated |
| 01 | Storage Security Features | **Complete** — encryption, HTTPS, CORS, RBAC, logging validated |
| 02 | Storage Account Keys | Shared Key risk, rotation, migration |
| 03 | Shared Access Signatures | Temporary delegated access and least privilege |

## Validation Model

Each lab follows the same pattern:

```text
CONFIGURE
   |
TEST
   |
OBSERVE
   |
CAPTURE EVIDENCE
   |
EXPLAIN THE SECURITY IMPACT
```

The strongest proof is not that a setting exists. It is that the expected behavior is observed.

Examples:

```text
Claims container         -> Allowed
Human Resources container -> Denied
```

```text
Old Storage Key -> Rejected after rotation
Secondary Key   -> Continues to work
```

```text
Valid SAS       -> Access succeeds
Expired SAS     -> Access fails
```

## Evidence

Each lab contains an `evidence/screenshots/` folder and a screenshot checklist.

Screenshots added to this repository should be:

- captured from the actual lab execution
- cropped to the relevant control or result
- free of passwords, Storage account keys, SAS tokens, session information, or unnecessary tenant/account identifiers
- named clearly
- referenced from the lab README

## Human-Pass Standard

This repository documents **hands-on learning in a training environment**. It does not claim production implementation experience where none occurred.

Before marking a lab complete:

- perform the exercise yourself
- record the actual result
- include sanitized evidence
- document any troubleshooting
- be able to explain the control without reading the README

## Security Findings to Validate

During the labs, validate observations such as:

- inherited Azure permissions can make an RBAC test misleading
- management-plane access and data-plane access are different
- Storage account keys provide broad access and must be protected
- rotating a key affects every client still using the old value
- user delegation SAS reduces reliance on account keys
- delegated access should use minimum permission, minimum scope, and minimum duration
- encryption alone does not establish authorization

## Business Impact

These controls reduce the chance that sensitive cloud data is exposed through overly broad permissions, long-lived secrets, weak key-management practices, or unnecessarily powerful delegated access.

For a data-protection program, the technical control is only part of the job. The engineer must also validate scope, understand dependencies, prove behavior, and preserve evidence.

## Key Takeaways

```text
Identity answers: Who are you?
RBAC answers: What can you do, and where?
Storage keys act like powerful shared secrets.
SAS provides temporary delegated access.
Logging provides evidence of what happened.
```

## Repository Safety

No production data or real customer information belongs in this repository.

Never commit:

- Storage account keys
- SAS URLs or SAS tokens
- client secrets
- passwords
- access tokens
- tenant-specific sensitive identifiers
- raw screenshots containing secrets

See `docs/PUBLICATION_CHECKLIST.md` before making the repository public.
