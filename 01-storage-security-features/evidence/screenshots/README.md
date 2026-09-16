# Screenshot Evidence Checklist

Add screenshots only after performing the lab. Sanitize all secrets and unnecessary account identifiers.

Recommended evidence:

- `01-encryption-at-rest.png` — Azure Storage encryption configuration.
- `02-secure-transfer.png` — Secure transfer / HTTPS requirement enabled.
- `03-cors-configuration.png` — Blob CORS rule for the controlled test origin.
- `04-rbac-claims-scope.png` — Claims-scoped Storage Blob Data Reader assignment.
- `05-claims-success.png` — Claims access success with Entra authentication.
- `06-humanresources-denied.png` — Human Resources access denied for the restricted Claims identity.
- `07-storage-logging.png` — Diagnostic/log configuration or actual storage log evidence.

Do not capture or commit full Storage keys, SAS tokens, passwords, access tokens, or session information.
