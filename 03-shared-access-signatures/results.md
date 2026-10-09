# Lab 03 — Observed results

**Execution date:** September 23, 2026. **Hands-on work: complete.** Evidence is from actual portal, Cloud Shell and Log Analytics results shown during the lab.

| Checkpoint | Observed result | Evidence |
|---|---|---|
| Storage containers | Existing private `claims` container reused; new private `claims-upload` created | [01](evidence/screenshots/01-private-containers.png) |
| Source inputs | `claim_summary_CL-6201.txt` and `claim_estimate_CL-6202.txt` added to existing `claims` container | [02](evidence/screenshots/02-synthetic-claims-in-existing-container.png) |
| Repair-partner SAS | Blob-level service SAS from portal, Key 1, read permission, HTTPS only, short expiration | [03](evidence/screenshots/03-blob-read-sas-settings-before-generation.png) |
| Read approved blob | `claim_summary_CL-6201.txt` displayed in a separate Incognito window using the SAS URL | [04](evidence/screenshots/04-approved-blob-read-success.png) |
| Try different blob | Changing path to `claim_estimate_CL-6202.txt` with the same query produced `AuthenticationFailed`, signature mismatch | [05](evidence/screenshots/05-different-blob-signature-mismatch.png) |
| Read SAS expiration | Previously working read link failed on refresh after the expiry (`Signature not valid in the specified time frame`) | [04](evidence/screenshots/04-approved-blob-read-success.png), [06](evidence/screenshots/06-blob-sas-expired-denied.png) |
| Customer-upload SAS | **Container-level**, account-key-signed service SAS: Create + Write only, HTTPS only; not limited to one blob name | [07](evidence/screenshots/07-upload-sas-create-write-settings.png) |
| First synthetic upload | `claim_photo_CL-6101.txt` uploaded with SAS in Cloud Shell; command completed successfully | [08](evidence/screenshots/08-first-sas-upload-success.png) |
| Attempted download with upload SAS | Denied | [09](evidence/screenshots/09-upload-sas-read-denied.png) |
| Attempted list with upload SAS | Denied | [10](evidence/screenshots/10-upload-sas-list-denied.png) |
| Attempted delete with upload SAS | Denied; file remained present in the Azure portal | [11](evidence/screenshots/11-upload-sas-delete-denied.png), [12](evidence/screenshots/12-uploaded-file-still-present.png) |
| SAS read-related audit | SAS requests and permission mismatches appeared in `StorageBlobLogs` with URI excluded from evidence | [13](evidence/screenshots/13-sas-read-logging-no-uri.png) |
| Write-log troubleshooting | First search for `PutBlob`/`PutBlock`/`PutBlockList` had no results. Storage Write was unchecked in the diagnostic setting. Enabled Storage Write before retest. | [14](evidence/screenshots/14-storage-read-write-diagnostics-enabled.png) |
| Second synthetic upload | Uploaded `customer_submission_note_CL-6101.txt` with a **fresh** Create + Write SAS | [15](evidence/screenshots/15-second-sas-upload-success.png) |
| Write audit | `PutBlob`, `AuthenticationType = SAS`, status `201 Success` appeared after retest | [16](evidence/screenshots/16-sas-putblob-201-audit-log.png) |
| Shell secret cleanup | Ran `unset UPLOAD_SAS` after second upload; shell returned to prompt | [17](evidence/screenshots/17-sas-shell-variable-cleared.png) |
| Final blob check | Both synthetic uploads visible in private `claims-upload` | [18](evidence/screenshots/18-both-synthetic-uploads-visible.png) |

## Boundaries and caveats

- **Actual Azure container name:** `claims`. The `synthetic-data/claims-read/` directory is just a local folder. No separate `claims-read` container was created in this run.
- The upload SAS was **container-scoped**. Create + Write alone does not guarantee that an upload client cannot overwrite an existing object. Overwrite resistance and a one-blob upload SAS were **not tested** here.
- The permission-denied Cloud Shell messages were generic; SAS `403 AuthorizationPermissionMismatch` events also appeared in Logs. Do not confuse successful *portal* SAS requests with access granted by the specifically restricted customer-upload SAS.
- Storage Write was added after the first upload, so that original upload was not recovered as a historical write event. The second upload is the one independently evidenced by `PutBlob` 201.
- A user delegation SAS, stored access policy, token revocation before expiry and the upload-token expiry test were **not performed**. The *read-only blob SAS* expiry test was performed.
- The first read link and both upload SAS credentials were left to their short signed expiration; the shell variable was cleared, and no credentials were saved to the repo.
