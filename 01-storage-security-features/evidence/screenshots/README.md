# Lab 01 Screenshot Evidence

These screenshots were captured during the live Azure exercise and sanitized for public GitHub use.

| File | What it proves |
|---|---|
| `01-encryption-at-rest-microsoft-managed-keys.png` | Storage encryption at rest is active with Microsoft-managed keys |
| `02-secure-transfer-required.png` | Secure transfer is enabled |
| `03-minimum-tls-12.png` | Minimum TLS version is 1.2 |
| `04-cors-baseline-no-rules.png` | Blob CORS had no rule before the test |
| `05-cors-allowed-origin-localhost8000.png` | The intended Blob CORS origin and GET method are configured |
| `06-cors-positive-test-http200.png` | Allowed browser origin successfully read the synthetic blob |
| `07-cors-negative-test-origin-blocked.png` | Wrong browser origin was blocked in a fresh session |
| `08-rbac-claims-allowed.png` | Claims Reader could list the Claims blob with Entra authentication |
| `09-rbac-humanresources-denied.png` | The same restricted identity was denied access to HR |
| `10-blob-read-diagnostic-setting.png` | Blob Storage Read logs are routed to Log Analytics |
| `11-log-analytics-getblob-oauth-success.png` | Log Analytics captured the successful OAuth/HTTPS GetBlob event |

No secrets are included. The SAS URL used during the browser test was short-lived and is not stored here.
