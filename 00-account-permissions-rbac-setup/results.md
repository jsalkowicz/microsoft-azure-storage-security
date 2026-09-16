# Lab 00 Results

## Environment

- Azure subscription: active
- Training resource group: created
- Training Storage account: created with Standard performance and LRS
- Secure transfer: enabled
- Anonymous Blob access: disabled
- Storage account key access: left enabled because later labs will test Shared Key behavior

## Administrative access

The lab-builder account had **Owner** at the subscription scope, inherited by the Storage account.

That confirmed I could create resources and role assignments, but it also meant I needed a separate restricted identity for a meaningful RBAC test.

## Test identity

- Display name: `Northstar Claims Reader`
- Entra admin role: none assigned
- Azure Blob role: `Storage Blob Data Reader`
- Scope: `claims` container only

## Data used

- `claims/claim_CL-7001.txt`
- `humanresources/employee_EMP-7001.txt`

Both files contain synthetic training data only.

## Validation

### Claims

```bash
az storage blob list   --account-name "$STORAGE_ACCOUNT"   --container-name claims   --auth-mode login   --output table
```

**Observed result:** Success. The test identity could see `claim_CL-7001.txt`.

### Human Resources

```bash
az storage blob list   --account-name "$STORAGE_ACCOUNT"   --container-name humanresources   --auth-mode login   --output table
```

**Observed result:** Authorization denied.

Azure reported that the signed-in identity did not have the required data permissions for the operation.

## Conclusion

The same Microsoft Entra identity could read the Claims container but could not read the Human Resources container.

**Lab 00 passed.**
