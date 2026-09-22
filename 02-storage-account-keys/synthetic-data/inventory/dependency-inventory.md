# Example dependency inventory (fictional)

I used this sample to think through how a key rotation would affect a legacy application. **These are not real deployed workloads and none was migrated during Lab 02.**

| Example workload | Data it needs | Example current authentication | What I would check before rotating |
|---|---|---|---|
| `claims-batch-export` | Read Claims | Key 1 | Confirm Key 2 works; move its credential reference and verify reads. |
| `claims-reader-modern` | Read Claims | Entra ID + container RBAC | Verify it does not use an account key or key-signed SAS. |
| `hr-reporting-demo` | Read HR | Entra ID + HR RBAC | Verify the separate HR scope; not part of the Claims Reader test. |

The `claims-batch-export` example needs one container, but a storage account key can reach both. That is the access-scope difference tested in this lab.
