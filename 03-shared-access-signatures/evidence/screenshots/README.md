# Lab 03 screenshot evidence — reviewed and cropped

All images below are based on actual screenshots shared during the September 23, 2026 lab. They are **cropped** to keep the relevant test result and omit browser address bars, signed query strings, irrelevant account headers and unrelated UI. No visible SAS token, signed SAS URL, account-key value or connection string is included. Do not upload unreviewed source screenshots containing addresses or credential-bearing log `Uri` values.

The screenshots demonstrate **what occurred**, not additional tests. The first read-success screenshot shows a synthetic document, not an active credential. The read/list success rows in general audit logs may come from the Azure portal rather than the restricted customer-upload SAS; see `results.md`.

| File | Verified checkpoint |
|---|---|
| [01-private-containers.png](01-private-containers.png) | Private containers present |
| [02-synthetic-claims-in-existing-container.png](02-synthetic-claims-in-existing-container.png) | Two Lab 03 source files in existing claims container |
| [03-blob-read-sas-settings-before-generation.png](03-blob-read-sas-settings-before-generation.png) | One-blob read SAS settings before generating |
| [04-approved-blob-read-success.png](04-approved-blob-read-success.png) | Approved claim summary displayed via read SAS |
| [05-different-blob-signature-mismatch.png](05-different-blob-signature-mismatch.png) | Different claim file rejected by same SAS signature |
| [06-blob-sas-expired-denied.png](06-blob-sas-expired-denied.png) | Original read SAS rejected after expiration |
| [07-upload-sas-create-write-settings.png](07-upload-sas-create-write-settings.png) | Create + Write-only container SAS settings |
| [08-first-sas-upload-success.png](08-first-sas-upload-success.png) | First upload succeeded using SAS |
| [09-upload-sas-read-denied.png](09-upload-sas-read-denied.png) | Read with upload SAS denied |
| [10-upload-sas-list-denied.png](10-upload-sas-list-denied.png) | List with upload SAS denied |
| [11-upload-sas-delete-denied.png](11-upload-sas-delete-denied.png) | Delete with upload SAS denied |
| [12-uploaded-file-still-present.png](12-uploaded-file-still-present.png) | First upload remained after denied delete |
| [13-sas-read-logging-no-uri.png](13-sas-read-logging-no-uri.png) | SAS-authenticated read/list and 403 events, no URI column |
| [14-storage-read-write-diagnostics-enabled.png](14-storage-read-write-diagnostics-enabled.png) | Storage Read + Storage Write categories enabled |
| [15-second-sas-upload-success.png](15-second-sas-upload-success.png) | Second SAS upload succeeded after write logging enabled |
| [16-sas-putblob-201-audit-log.png](16-sas-putblob-201-audit-log.png) | PutBlob / SAS / 201 Success recorded |
| [17-sas-shell-variable-cleared.png](17-sas-shell-variable-cleared.png) | Upload SAS variable cleared from shell |
| [18-both-synthetic-uploads-visible.png](18-both-synthetic-uploads-visible.png) | Both submitted synthetic files present |

**Limit:** Some screenshots show only generic CLI permission errors. Interpretation is supported by the configured SAS permissions and the matching audit events, not by assuming every CLI error message names the exact cause.
