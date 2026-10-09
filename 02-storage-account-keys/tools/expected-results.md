# Expected versus observed

This lab completed its core checks in September 2026. See [the actual test record](../results.md) and the evidence index instead of treating the commands below as synthetic output.

| Check | Expected | Observed |
|---|---|---|
| Key 1 across Claims and HR | Both allowed | Both allowed |
| Key 2 before rotation | Allowed | Allowed |
| Previous Key 1 after regeneration | Denied | Authentication failure |
| Fresh Key 1 after regeneration | Allowed | Allowed |
| Current key with Shared Key disabled | Denied | `KeyBasedAuthenticationNotPermitted` |
| Scoped Entra reader in Claims / HR | Allowed / denied | Allowed / denied |
| Audit trail | `AccountKey` records | Four successful `ListBlobs` rows in the screenshot |
