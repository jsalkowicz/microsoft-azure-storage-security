# Lab 03 — Azure Shared Access Signatures

## Security Objective

Practice temporary delegated access to Azure Storage without handing an external client a Storage account key.

## Security Problem

External or untrusted clients may need direct access to a Blob for a limited business task. Giving them a Storage account key would provide unnecessarily broad access.

A SAS can constrain:

```text
WHAT can the client do?
WHERE can it do it?
HOW LONG can it do it?
```

## Implementation

Follow `lab-guide.md`.

The lab covers:

- short-lived Blob SAS access
- user delegation SAS
- read/write permission selection
- Blob/resource scope
- expiration behavior
- SAS versus managed identity

## Validation

Evidence should show:

- valid short-lived SAS access succeeds
- access fails after expiration
- permission choice matches the business requirement
- resource scope is intentionally limited
- a user delegation SAS can be created using Entra authentication when supported

## Security Finding

A SAS is still a bearer secret. Whoever possesses a valid SAS URI may be able to exercise the permissions encoded in it until it expires or is invalidated.

## Business Impact

Short-lived, narrowly scoped delegated access reduces the need to distribute powerful long-lived account credentials to external clients.

## Evidence

Never commit a live SAS token or complete SAS URL.

See `evidence/screenshots/README.md`.

## Status

**In Progress** until the lab is performed and sanitized validation evidence is added.
