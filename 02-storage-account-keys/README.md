# Lab 02 — Storage account keys

**Status:** Completed in the training tenant, September 2026  
**Data:** Synthetic claims and HR text files only

## What I set out to test

In Lab 01, I gave a test user Blob Data Reader access to the `claims` container, but not `humanresources`. For this lab, I wanted to see how that scoped access compares with using a storage account key. I also wanted to work through a two-key rotation and test what happens when Shared Key authorization is turned off.

This was a hands-on lab in my own Azure training account, not a production migration. The fictional Northstar workload in `synthetic-data/inventory/` is there to explain the scenario; no real application was moved between keys.

## What I did

| Test | Observed result | Evidence |
|---|---|---|
| Key 1 against Claims and HR | Both container listings succeeded; the same account-level credential reached both containers. | [Key 1 / HR](evidence/screenshots/02-key1-humanresources-access.png); Claims was confirmed during the session, but its separate screenshot is not in this package. |
| Key 2 before rotation | Listed the Claims blobs successfully. | [Key 2 test](evidence/screenshots/03-key2-claims-access.png) |
| Regenerate Key 1 | The renew command completed without an error. | [Rotation command](evidence/screenshots/04-key1-rotation-command.png) |
| Old versus new Key 1 | Old value failed authentication; refreshed value listed Claims successfully. | [Old value rejected](evidence/screenshots/05-old-key1-authentication-failure.png) · [New value accepted](evidence/screenshots/06-new-key1-claims-access.png) |
| Blob access logging | `StorageBlobLogs` returned successful `ListBlobs` entries using `AccountKey` for Claims and HR. | [Log Analytics](evidence/screenshots/07-log-analytics-accountkey-audit.png) |
| Disable Shared Key authorization | A valid, current account key returned `KeyBasedAuthenticationNotPermitted`. | [Setting disabled](evidence/screenshots/08-shared-key-disabled.png) · [Key rejected](evidence/screenshots/09-valid-key-blocked-shared-key-disabled.png) |
| Test container-scoped Entra/RBAC | The Claims Reader could list Claims but was denied access to HR with `--auth-mode login`. | [Claims allowed](evidence/screenshots/10-claimsreader-claims-entra-success.png) · [HR denied](evidence/screenshots/11-claimsreader-humanresources-entra-denied.png) |
| Restore the lab | Re-enabled Shared Key access for the next SAS lab and cleared temporary shell key variables. | [Setting restored](evidence/screenshots/12-shared-key-reenabled.png) · [Variables cleared](evidence/screenshots/13-shell-key-variables-unset.png) |

## What I took away

The account key worked across both containers, while the restricted identity was limited to its RBAC assignment. Testing Key 2 before regenerating Key 1 gave me a fallback path, and the old-key failure verified that the rotation actually took effect. Turning off Shared Key blocked a valid key without blocking the Claims Reader's Entra-based access.

One operational detail stood out: having permission to manage a storage account is not the same as having Blob data permissions. The admin Cloud Shell identity could manage the account but was denied a Blob listing with `--auth-mode login`. I used the existing Claims Reader identity for the Entra validation instead of granting the admin additional data access just to pass the test.

## Files in this lab

- [Lab steps and commands](lab-guide.md)
- [Observed results and limitations](results.md)
- [Key rotation runbook](docs/key-rotation-runbook.md)
- [Screenshot evidence index](evidence/screenshots/README.md)
- [Audit query](tools/log-analytics-queries.kql)
- [Synthetic data and example dependency inventory](synthetic-data/)

**Security note:** Screenshots have browser address bars and signed-in identity headers removed or masked. No account-key values, passwords, connection strings, or SAS tokens are included. Resource names and synthetic file names are kept so the testing evidence is readable. These images are edited copies of the original lab screenshots.
