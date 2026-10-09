# Lab 01 — Reproduction Notes

These are the steps I used for the live Azure lab.

## 1. Verify encryption at rest

Open the Storage account and go to **Encryption**.

Confirm the active encryption type. In this lab the account used Microsoft-managed keys.

## 2. Verify secure transfer and TLS

Go to **Settings -> Configuration**.

Confirm:

- Secure transfer required = Enabled
- Minimum TLS version = 1.2

## 3. Test Blob CORS

Go to **Settings -> Resource sharing (CORS)** and use the Blob service.

Configure:

```text
Allowed origin: http://localhost:8000
Allowed method: GET
```

Serve `tools/cors_test.html` locally:

```bash
python -m http.server 8000
```

Open:

```text
http://localhost:8000/cors_test.html
```

For the test only, create a short-lived read-only SAS URL for the synthetic claim blob. Do not save the SAS in source control.

Expected positive test:

```text
SUCCESS
HTTP 200
```

Then change the allowed origin to `http://localhost:8001` and retest from a fresh browser session at `localhost:8000`.

Expected negative test:

```text
BROWSER REQUEST FAILED
TypeError: Failed to fetch
```

Restore the intended rule to `http://localhost:8000` after testing.

## 4. Validate Entra ID + RBAC

Use the restricted training identity and force Entra authentication with `--auth-mode login`.

Claims test:

```bash
STORAGE_ACCOUNT="northstarlab48217"

az storage blob list \
  --account-name "$STORAGE_ACCOUNT" \
  --container-name claims \
  --auth-mode login \
  --output table
```

Expected: `claim_CL-7001.txt` is listed.

HR test:

```bash
az storage blob list \
  --account-name "$STORAGE_ACCOUNT" \
  --container-name humanresources \
  --auth-mode login \
  --output table
```

Expected: permission denied.

## 5. Enable Blob read diagnostics

Open the Blob service diagnostic settings and create:

```text
Diagnostic setting: blob-read-audit
Category: Storage Read
Destination: Log Analytics workspace
```

The workspace used in this lab was:

```text
law-northstar-storage-labs
```

Generate a new read after the diagnostic setting is active:

```bash
az storage blob download \
  --account-name northstarlab48217 \
  --container-name claims \
  --name claim_CL-7001.txt \
  --file /tmp/claim_CL-7001.txt \
  --auth-mode login \
  --overwrite
```

Then query Log Analytics:

```kusto
StorageBlobLogs
| where TimeGenerated > ago(1h)
| order by TimeGenerated desc
```

Observed result:

```text
OperationName: GetBlob
AuthenticationType: OAuth
Protocol: HTTPS
StatusText: Success
```
