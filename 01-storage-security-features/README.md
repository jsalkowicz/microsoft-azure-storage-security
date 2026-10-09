# Lab 01 — Azure Storage Security Features

**Status:** Complete  
**Environment:** Hands-on Azure training environment using synthetic insurance data only

## What I wanted to test

This lab was meant to separate several storage security controls that are easy to mix together:

- encryption at rest
- secure transfer and TLS
- browser-origin controls with CORS
- Microsoft Entra ID and Azure RBAC
- Blob access logging

The main goal was to prove the controls with actual allowed and denied behavior instead of stopping at the configuration screen.

## What I did

I used the Storage account created in Lab 00 and worked through five controls.

First, I verified that Azure Storage encryption was active with Microsoft-managed keys. I then confirmed that secure transfer was required and the minimum TLS version was 1.2.

For CORS, I allowed `http://localhost:8000` to make `GET` requests to Blob Storage. I used a short-lived read-only SAS URL only for the browser test. The SAS value was not saved in this repository. The request succeeded from the allowed origin with HTTP 200. I then changed the allowed origin to `http://localhost:8001` and repeated the same test from `localhost:8000`. A fresh Incognito session failed with `TypeError: Failed to fetch`, confirming that the browser origin was being enforced.

For RBAC, I reused the `Northstar Claims Reader` training identity from Lab 00. Using `--auth-mode login`, the identity could list the `claims` container but was denied access to `humanresources`.

Finally, I enabled Blob `Storage Read` diagnostics and sent them to a Log Analytics workspace. After generating a fresh Blob download, the `StorageBlobLogs` table recorded a successful `GetBlob` operation using OAuth over HTTPS.

## Results

| Control | Expected result | Observed result | Status |
|---|---|---|---|
| Encryption at rest | Storage data is encrypted | Microsoft-managed keys shown | Pass |
| Secure transfer | HTTPS required | Secure transfer enabled | Pass |
| Minimum TLS | TLS 1.2 or higher | TLS 1.2 configured | Pass |
| CORS allowed origin | `localhost:8000` can read with valid authorization | HTTP 200 | Pass |
| CORS wrong origin | `localhost:8000` blocked when only `8001` is allowed | Browser fetch failed | Pass |
| Claims RBAC | Claims Reader can access Claims | Blob list succeeded | Pass |
| HR RBAC | Claims Reader cannot access HR | Permission denied | Pass |
| Blob auditing | Read activity is captured | `GetBlob`, OAuth, HTTPS, Success | Pass |

## Troubleshooting note

The first CORS negative test still returned the blob in the existing browser session after I changed the allowed origin. I repeated the test in a fresh Incognito session and received the expected browser failure.

That was a useful reminder that cached browser behavior can make a control test look misleading if the test method is not reset between changes.

## What this showed me

Encryption protects stored data, but it does not decide who is authorized to read it.

CORS is also not authorization. The SAS authorized access to the private blob for the browser test, while CORS separately controlled whether that browser origin was allowed to make the request.

RBAC scope mattered. A role assigned at the `claims` container allowed that data while leaving `humanresources` denied.

Logging completed the validation by giving me evidence of the actual access event instead of only the configured policy.

## Memory map

```text
AT REST -> IN TRANSIT -> BROWSER -> ACCESS -> AUDIT
SSE     -> HTTPS      -> CORS    -> RBAC   -> LOGS
```

## Evidence

Sanitized screenshots are under `evidence/screenshots/`.

The screenshots show the actual lab results. Account-specific browser chrome, tenant details, and unnecessary identifiers were cropped out where possible. No Storage account keys, SAS tokens, passwords, or access tokens are included.

## Safety

All claim and employee records in this lab are synthetic training data. No production or customer data is included.
