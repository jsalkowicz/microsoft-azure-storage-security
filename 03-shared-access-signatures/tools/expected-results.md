# Expected and observed behavior

See [results.md](../results.md) for the live outcome and evidence for every completed test.

| Action | Expected | Observed |
|---|---|---|
| Read original blob with its read SAS before expiry | Allowed | Allowed |
| Change URL path to a different blob using the same SAS | Denied | Denied: signature mismatch |
| Refresh the original URL after expiry | Denied | Denied: signature outside valid time frame |
| Upload new blob using container SAS with Create + Write | Allowed | Allowed (two different synthetic files, with separate tokens) |
| Read, list, delete with upload SAS | Denied | Denied for all three tests |
| Query write logs before Storage Write diagnostics were enabled | No retroactive write record | No matching upload record found |
| Query write logs after enabling Storage Write and performing a new SAS upload | New write event | `PutBlob` / `SAS` / `201 Success` |

**Not tested:** user delegation SAS, truly single-blob upload SAS, overwrite attack, premature SAS revocation, and upload-SAS expiry. Do not describe those as completed.
