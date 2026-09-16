# Screenshot Evidence Checklist

Add screenshots only after performing the lab. Sanitize all secrets and unnecessary account identifiers.

Recommended evidence:

- `01-sas-configuration-redacted.png` — SAS permission/scope/expiry configuration with token values omitted.
- `02-valid-sas-access.png` — Successful access using a short-lived SAS without exposing the full URI.
- `03-expired-sas-denied.png` — Access failure after SAS expiration.
- `04-user-delegation-sas-command.png` — Cloud Shell command/result showing user delegation flow with SAS output fully redacted.

Do not capture or commit full Storage keys, SAS tokens, passwords, access tokens, or session information.
