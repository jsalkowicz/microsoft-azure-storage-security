# Lab 00 Screenshot Evidence

These screenshots were captured during the live Azure exercise.

Redactions were applied only where the original image exposed information that was not needed to demonstrate the control, including personal account details, tenant information, object identifiers, or the live Storage account name.

| File | Evidence |
|---|---|
| `01-resource-group-created.png` | Training resource group created in Azure. |
| `02-storage-account-created-redacted.png` | Training Storage account created in the lab resource group. |
| `03-admin-owner-inherited-redacted.png` | Lab-builder account had Owner inherited from subscription scope. |
| `04-test-user-created-redacted.png` | Separate Northstar Claims Reader Entra identity created. |
| `05-containers-created-redacted.png` | Private `claims` and `humanresources` containers created. |
| `06-storage-blob-data-reader-selected.png` | Storage Blob Data Reader role selected for the test identity. |
| `07-claims-rbac-assignment-redacted.png` | Claims-scoped RBAC assignment confirmed. |
| `08-claims-allowed-redacted.png` | Claims Blob listing succeeded using `--auth-mode login`. |
| `09-humanresources-denied-redacted.png` | Human Resources Blob listing was denied for the same identity. |

Raw screenshots are intentionally not included in the public repository.
