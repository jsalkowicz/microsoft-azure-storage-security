# Lab 01 — Azure Storage Security Features

## Security Objective

Configure and validate several core Azure Storage security controls:

- encryption at rest
- HTTPS / secure transfer
- Blob CORS
- Microsoft Entra ID + Azure RBAC
- storage activity logging

## Security Problem

Storage can be encrypted while still being overexposed. This lab separates confidentiality controls from access controls and proves the difference through testing.

## Implementation

Follow `lab-guide.md`.

Synthetic files are under `synthetic-data/`.

The CORS test page is under:

`tools/cors_test.html`

## Validation

Key validation points:

```text
Encryption at rest = enabled
Secure transfer    = required
Claims identity    = allowed to Claims
Claims identity    = denied from HR
CORS               = affects browser cross-origin behavior
Logging            = captures storage activity
```

## Security Finding

A configured security setting is not sufficient evidence by itself. The control should be tested against expected allowed and denied behavior.

## Business Impact

These controls reduce exposure of sensitive Blob data by combining encryption, transport protection, authorization, browser-origin restrictions, and monitoring.

## Evidence

The repository includes a synthetic log for analysis, but portfolio evidence should also include sanitized screenshots from the live Azure exercise.

See `evidence/screenshots/README.md`.

## Status

**In Progress** until actual Azure results and screenshots are added.
