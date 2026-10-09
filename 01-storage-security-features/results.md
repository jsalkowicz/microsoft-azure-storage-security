# Lab 01 Results

## Final status

**Complete**

All five control areas were configured or verified and then tested.

```text
Encryption at rest       PASS
Encryption in transit    PASS
CORS                      PASS
Entra ID + RBAC           PASS
Storage access auditing   PASS
```

## Key observations

- Microsoft-managed encryption was active for the Storage account.
- Secure transfer was required and minimum TLS was 1.2.
- CORS allowed `localhost:8000` and blocked that same origin when the rule was changed to `localhost:8001`.
- The Claims Reader identity could access `claims` and could not access `humanresources`.
- A fresh Blob download appeared in `StorageBlobLogs` as `GetBlob` with OAuth, HTTPS, and Success.

## Repository note

The browser test used a temporary read-only SAS because the blob was private. The SAS was used only during the test and was not written to any repository file.
