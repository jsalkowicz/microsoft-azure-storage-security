# Storage account key rotation — lab runbook

I used this checklist to structure the Key 1 rotation in Lab 02. It is a training example, not a completed production change record.

1. Check which workloads use Key 1, and whether they also use account SAS tokens signed by that key. Regeneration can break those dependencies.
2. Verify Key 2 before touching Key 1.
3. Move any *real* Key 1 consumers to Key 2, then confirm they still work. **Lab note:** I did not perform this cutover; the example workload in the inventory is fictional.
4. Regenerate Key 1 only after the dependencies have been moved or confirmed absent.
5. Confirm the old value fails and the new value succeeds, without printing either value.
6. Update dependent consumers to the new value if required; rotate Key 2 only as a separate, planned change.
7. Remove temporary copies of the keys from the working session. Check that no credentials were committed to source control or saved in evidence.

## What I verified here

Key 2 worked before the renewal; old Key 1 failed after it; new Key 1 worked. The storage account was a training account without real application dependencies, so this lab demonstrates the mechanics of rotation **rather than** a zero-downtime production migration.

For a new workload, I would investigate Microsoft Entra authentication and scoped RBAC first so the workload does not need an account-level secret in the first place.
