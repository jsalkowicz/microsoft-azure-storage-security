# Lab 03 — Execution guide and test record

This records the actual September 23, 2026 run. Use the [results](results.md) and [evidence index](evidence/screenshots/README.md) to check each observed result. Do not interpret an expected outcome as completed evidence.

## 1. Prepare data

In the training storage account, I reused the private `claims` container. I uploaded the two invented local files from `synthetic-data/claims-read/` directly into **`claims`**. I did **not** create a separate Azure `claims-read` container. I created a different private container named `claims-upload` for customer-submission tests.

## 2. Read-only blob SAS

I opened `claims/claim_summary_CL-6201.txt` in the portal, selected Generate SAS and used account key signing (Key 1), Read permission and HTTPS only. I set an approximately one-hour lifetime. I copied the generated SAS URL locally, opened it in a separate Incognito window and observed the synthetic summary displayed. I did not record the URL or token.

Changing **only the filename** in the URL to `claim_estimate_CL-6202.txt` while keeping the same SAS query returned `AuthenticationFailed` / signature mismatch. Refreshing the original approved link *after* its signed expiration returned a time-frame failure. These are distinct negative tests: wrong signed resource versus expired credential.

## 3. Container-level customer-upload SAS

I created private `claims-upload` and left it empty before the first test. From the container's `... → Generate SAS` menu, I selected **Create + Write only**, with HTTPS only and a short expiry. This was a **container-level** SAS, not a one-blob SAS. Read, List and Delete were not selected.

I copied only the token into a non-echoed shell variable and uploaded the local synthetic `claim_photo_CL-6101.txt` using `az storage blob upload` with `--sas-token "$UPLOAD_SAS"`. The upload succeeded. Attempts with the same SAS to download the blob, list the container or delete the blob were denied. A portal check confirmed the first upload remained in place.

## 4. Audit the operations

The existing Blob diagnostic setting originally had **Storage Read** enabled but not **Storage Write**. SAS-related read/list requests and authorization-denied events appeared in Log Analytics, but a `PutBlob` query initially returned zero results.

I enabled Storage Write to the existing Log Analytics workspace, transferred the second synthetic file (`customer_submission_note_CL-6101.txt`) to Cloud Shell and generated a **fresh** Create + Write SAS for `claims-upload`. The second upload succeeded. Re-running the write query showed `PutBlob`, `SAS`, `201 Success`. Enabling diagnostics does not backfill earlier operations.

## 5. Cleanup and final check

I ran `unset UPLOAD_SAS` in the admin shell after the second upload. The final portal screenshot shows both synthetic files in the private upload container. The short-lived SAS credentials were not included in the repository.

## Security limits demonstrated

A **blob-level** read SAS signed for one file could not be moved to another blob by editing the URL path. In contrast, the **container-level** Create + Write SAS applied to the whole upload container and did not guarantee create-only or single-filename access. A production upload workflow would normally generate a signed URL for a specific target path or use a backend to enforce naming, overwrite and authorization rules.

A user delegation SAS was discussed but **not tested**. Repeated successful `ListBlobs` activity from the Azure portal should not be attributed to the restricted upload token without more evidence.
