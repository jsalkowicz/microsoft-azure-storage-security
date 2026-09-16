# Screenshot Evidence Checklist

Add screenshots only after performing the lab. Sanitize all secrets and unnecessary account identifiers.

Recommended evidence:

- `01-access-keys-page-redacted.png` — Access Keys page showing Key 1 / Key 2 controls with all key values fully redacted.
- `02-key1-auth-success.png` — Shared Key authentication success without exposing the key.
- `03-key2-auth-success.png` — Secondary key successfully used before rotation.
- `04-old-key1-rejected.png` — Old Key 1 value rejected after regeneration.
- `05-entra-auth-success.png` — Blob operation using --auth-mode login.
- `06-shared-key-disabled.png` — Allow storage account key access disabled, if the optional step is performed.

Do not capture or commit full Storage keys, SAS tokens, passwords, access tokens, or session information.
