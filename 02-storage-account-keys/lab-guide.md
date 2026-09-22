# Lab 02 — What I ran

This is the order I followed in the training tenant. I used one admin session for key management and a separate **Claims Reader** session for the RBAC test. All blob content is synthetic. **Do not use the rotation section on a real storage account without inventorying dependencies and planning a cutover.**

## 1. Baseline: compare account keys with RBAC

Under **Storage account → Settings → Configuration**, I confirmed **Allow storage account key access = Enabled**. Azure showed two access keys. In the admin Cloud Shell, I used temporary variables rather than displaying or pasting credential values:

```bash
RG="rg-northstar-storage-labs"
STORAGE_ACCOUNT="northstarlab48217"
KEY1=$(az storage account keys list --resource-group "$RG" --account-name "$STORAGE_ACCOUNT" --query '[0].value' -o tsv)
```

```bash
az storage blob list --account-name "$STORAGE_ACCOUNT" --container-name claims --account-key "$KEY1" -o table
az storage blob list --account-name "$STORAGE_ACCOUNT" --container-name humanresources --account-key "$KEY1" -o table
```

Both listings succeeded. That gave me a direct comparison with the Claims Reader's container-scoped access from Lab 01.

## 2. Check the alternate key, then rotate Key 1

The storage account was only being used for these labs, so there were no actual application dependencies to migrate. I still tested Key 2 **before** regenerating Key 1, following the dependency-check principle in the [runbook](docs/key-rotation-runbook.md):

```bash
KEY2=$(az storage account keys list --resource-group "$RG" --account-name "$STORAGE_ACCOUNT" --query '[1].value' -o tsv)
az storage blob list --account-name "$STORAGE_ACCOUNT" --container-name claims --account-key "$KEY2" -o table
```

With Key 2 confirmed, I retained the old Key 1 only in the shell session, then renewed Key 1:

```bash
OLD_KEY1="$KEY1"
az storage account keys renew --resource-group "$RG" --account-name "$STORAGE_ACCOUNT" --key key1 --output none
```

The next request using `OLD_KEY1` failed authentication. I fetched the refreshed Key 1 and repeated the listing, which succeeded:

```bash
az storage blob list --account-name "$STORAGE_ACCOUNT" --container-name claims --account-key "$OLD_KEY1" -o table
NEW_KEY1=$(az storage account keys list --resource-group "$RG" --account-name "$STORAGE_ACCOUNT" --query '[0].value' -o tsv)
az storage blob list --account-name "$STORAGE_ACCOUNT" --container-name claims --account-key "$NEW_KEY1" -o table
```

## 3. Check the audit trail

I opened the lab's Log Analytics workspace and ran the [query in `tools/`](tools/log-analytics-queries.kql). The results contained successful `ListBlobs` operations with `AuthenticationType = AccountKey`, including operations against both containers.

## 4. Turn off Shared Key, then compare identities

In **Configuration**, I disabled **Allow storage account key access** and saved it. The current account key then failed:

```bash
az storage blob list --account-name "$STORAGE_ACCOUNT" --container-name claims --account-key "$NEW_KEY1" -o table
# ErrorCode: KeyBasedAuthenticationNotPermitted
```

Cloud Shell had restarted during this phase, so I restored `STORAGE_ACCOUNT` and re-fetched the current key before the final test. I did **not** count the first attempt with empty variables as proof of the control.

Next, I tried `--auth-mode login` using the admin shell. That identity did not have the Blob data role needed for a listing. I switched to the already-scoped Claims Reader identity for the actual Entra/RBAC comparison:

```bash
az storage blob list --account-name northstarlab48217 --container-name claims --auth-mode login -o table
az storage blob list --account-name northstarlab48217 --container-name humanresources --auth-mode login -o table
```

Claims listed successfully; Human Resources returned a permissions error. Shared Key remained disabled during those requests.

## 5. Leave the lab ready for the next exercise

I restored **Allow storage account key access = Enabled** in Configuration for the planned SAS lab, then cleared the temporary key variables in the admin Cloud Shell:

```bash
unset KEY1 KEY2 OLD_KEY1 NEW_KEY1
```

The actual credential values never appear in the command examples or the published evidence. Avoid running CLI commands with `--debug` when handling credentials; command-line arguments can also be exposed to local process inspection during execution. For a production system, use a managed identity and Azure RBAC where the application supports them.
