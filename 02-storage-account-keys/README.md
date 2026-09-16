# Lab 02 — Azure Storage Account Keys

## Security Objective

Understand the security impact of Shared Key authentication and practice safe Storage account key rotation.

## Security Problem

A Storage account key is a powerful shared secret. If the key is embedded in application configuration or exposed to an unauthorized party, the blast radius can be much larger than a narrowly scoped RBAC assignment.

## Implementation

Follow `lab-guide.md`.

The lab practices:

```text
FIND dependencies
-> SWITCH a test client to the other key
-> ROTATE the affected key
-> TEST that the old key fails
-> compare with Entra authentication
-> MIGRATE away from Shared Key where possible
```

## Validation

Evidence should prove:

- a valid account key can authenticate
- the secondary key can keep the test client operational
- the old Key 1 value fails after regeneration
- the other valid key continues to work
- Entra authentication does not require an account key
- Shared Key can be disabled after dependencies are migrated

## Security Finding

Key rotation is both a security operation and a production-change concern. A client that still uses the old key will fail after regeneration.

## Business Impact

Reducing dependency on shared account-level secrets limits credential exposure and enables more identity-based, scoped access patterns.

## Evidence

Never capture or commit the actual key values.

See `evidence/screenshots/README.md`.

## Status

**In Progress** until the real Azure tests are performed and sanitized results are added.
