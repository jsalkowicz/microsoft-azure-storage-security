# Lab 03 — Shared Access Signatures (SAS)

**Status: Completed — live tests performed September 23, 2026.**

## Why I ran this lab

I wanted to see what happens when a repair partner needs one claim summary and a customer needs to submit a document, but neither should receive general access to the claims storage account. I used synthetic insurance records only. The real test here was not simply creating a SAS token; it was trying permitted and denied operations and checking the resulting audit logs.

## Lab environment

- Training storage account: `northstarlab48217` in `rg-northstar-storage-labs`.
- Existing private `claims` container held the two Lab 03 repair-partner files. I **did not create an Azure container named `claims-read`**; that name is only the local source-data directory.
- Created a separate private `claims-upload` container for customer-submission tests. The existing `humanresources` container was not used or changed.
- Created SAS credentials in the Azure portal, signed with storage account Key 1. These were **service SAS** tokens, not account SAS or user delegation SAS.
- Reviewed records in `law-northstar-storage-labs`, table `StorageBlobLogs`.

## What I tested

| Test | Observed result | Evidence |
|---|---|---|
| Blob-level, read-only SAS opens the approved claim summary without an Azure sign-in | Allowed | [04](evidence/screenshots/04-approved-blob-read-success.png) |
| Changing just the blob filename to the separate claim estimate, while keeping the same SAS query | Rejected: signature did not match | [05](evidence/screenshots/05-different-blob-signature-mismatch.png) |
| Refreshing the original read link after its signed expiration | Rejected: signature not valid in the specified time frame | [06](evidence/screenshots/06-blob-sas-expired-denied.png) |
| Container-level SAS with Create + Write uploads a new synthetic claim file | Allowed | [08](evidence/screenshots/08-first-sas-upload-success.png) |
| Same upload SAS attempts to read, list and delete | All three requests denied | [09](evidence/screenshots/09-upload-sas-read-denied.png), [10](evidence/screenshots/10-upload-sas-list-denied.png), [11](evidence/screenshots/11-upload-sas-delete-denied.png) |
| Check whether the denied delete removed the first upload | File remained | [12](evidence/screenshots/12-uploaded-file-still-present.png) |
| Enable Storage Write diagnostics and upload a second synthetic file with a fresh SAS | Upload succeeded; `PutBlob`, `SAS`, `201 Success` appeared in Log Analytics | [15](evidence/screenshots/15-second-sas-upload-success.png), [16](evidence/screenshots/16-sas-putblob-201-audit-log.png) |
| Remove the upload SAS variable from the admin shell | `unset UPLOAD_SAS` returned to the prompt | [17](evidence/screenshots/17-sas-shell-variable-cleared.png) |

The final container view showed both uploaded synthetic documents. See [all observed results](results.md) and [evidence index](evidence/screenshots/README.md).

## What I learned

The read token was scoped to **one specific blob**, and the access window was enforced. The customer-upload token was different: I generated it for the **container**, with Create + Write but no Read, List or Delete. That token was not restricted to a single filename, and **Write is not the same thing as create-only**: a party holding a still-valid container-level token may also be able to modify an existing blob. For a real customer-upload workflow, I would narrow the resource and design the upload endpoint to prevent clients from choosing or overwriting other objects.

I also initially found no `PutBlob` record because the existing diagnostic setting had Storage Read enabled but **Storage Write disabled**. I enabled Storage Write and ran a second upload to produce new, verifiable write-log evidence. Enabling that category does not recreate historical logs. The generic `AuthenticationType = SAS` field does not identify which particular SAS token was used, and successful portal-generated SAS requests are not proof that my restricted upload token had List permission.

## Files

- [Working guide — actual sequence](lab-guide.md)
- [Results and limitations](results.md)
- [SAS design and incident notes](docs/sas-design-and-incident-notes.md)
- [Synthetic input files](synthetic-data/)
- [Cloud Shell commands](tools/cloud-shell-commands.md)
- [Audit queries](tools/log-analytics-queries.kql)
- [Screenshot evidence](evidence/screenshots/README.md)

**Evidence hygiene:** No SAS tokens, signed URLs, storage account keys, connection strings, or real claim records belong in this public repository. Screenshots have been cropped to omit browser addresses and credentials. Active SAS links were never committed.
