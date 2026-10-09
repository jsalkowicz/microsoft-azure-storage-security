# Lab 02 — Results

**Run:** September 18–22, 2026  
**Status:** Hands-on tests completed; documentation prepared for GitHub

## Test record

| Check | Result | Notes |
|---|---|---|
| Shared Key permitted at baseline | Pass | Configuration confirmed before the key tests. |
| Key 1 -> Claims | Pass | Blob listing worked in the admin session. Separate original screenshot was not included in the evidence archive. |
| Key 1 -> HR | Pass | The same key listed the two synthetic employee files. |
| Key 2 -> Claims | Pass | Alternate key verified before Key 1 was regenerated. |
| Regenerate Key 1 | Pass | Azure CLI returned to prompt with no error. |
| Previous Key 1 -> Claims | Expected failure | CLI reported an authentication failure. |
| New Key 1 -> Claims | Pass | Blob listing worked with the refreshed credential. |
| `AccountKey` audit events | Pass | Four successful `ListBlobs` rows appeared in the captured query result, across Claims and HR. |
| Shared Key disabled, current key -> Claims | Expected failure | `KeyBasedAuthenticationNotPermitted`. An initial attempt after Cloud Shell restarted had missing variables; the current key was reloaded and the test was repeated successfully. |
| Admin account using `--auth-mode login` | Expected permission denial for this identity | Admin had account management access, but not the Blob data role needed for this request. Not evidence that Entra authentication was disabled. |
| Claims Reader, Entra -> Claims | Pass | Both synthetic Claims blobs listed. |
| Claims Reader, Entra -> HR | Expected failure | Azure reported missing permissions. |
| Shared Key restored for Lab 03 | Pass | Configuration showed Enabled and success notification. |
| Temporary shell key variables cleared | Pass | `unset` completed in the admin session. |

## Limits of the exercise

The alternate key was tested, but there was **no actual application cutover**: the dependency inventory is fictional. The audit screenshot proves that account-key requests were logged; it does not identify which of the two keys signed each request. I did not try to assign the admin a broader data role, and I did not rotate Key 2.

The first Key 1 Claims listing was validated in the live session, but the screenshot for that individual action is not in this public package. The included Key 1 HR listing plus the Key 2 and new-Key 1 Claims listings cover the rest of the sequence. The portal's stuck `Loading…` view for the restricted account was a UI issue; I ran the Entra/RBAC comparison directly from that user's Cloud Shell.

## End state

Shared Key access was re-enabled for the planned Lab 03 SAS exercise, and the temporary shell key variables were unset. A longer-term design for a real workload would use Entra/RBAC where supported rather than relying on a broadly scoped account key.
