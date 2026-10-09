# Command reference

This is a quick reference to the commands I actually used. The [lab guide](../lab-guide.md) explains the order, failure tests, Cloud Shell session reset, and required precautions. Commands here use shell variables rather than literal secrets.

```bash
RG="rg-northstar-storage-labs"
STORAGE_ACCOUNT="northstarlab48217"
KEY1=$(az storage account keys list --resource-group "$RG" --account-name "$STORAGE_ACCOUNT" --query '[0].value' -o tsv)
az storage blob list --account-name "$STORAGE_ACCOUNT" --container-name claims --account-key "$KEY1" -o table
az storage blob list --account-name "$STORAGE_ACCOUNT" --container-name humanresources --account-key "$KEY1" -o table
KEY2=$(az storage account keys list --resource-group "$RG" --account-name "$STORAGE_ACCOUNT" --query '[1].value' -o tsv)
az storage blob list --account-name "$STORAGE_ACCOUNT" --container-name claims --account-key "$KEY2" -o table
OLD_KEY1="$KEY1"
az storage account keys renew --resource-group "$RG" --account-name "$STORAGE_ACCOUNT" --key key1 --output none
az storage blob list --account-name "$STORAGE_ACCOUNT" --container-name claims --account-key "$OLD_KEY1" -o table
NEW_KEY1=$(az storage account keys list --resource-group "$RG" --account-name "$STORAGE_ACCOUNT" --query '[0].value' -o tsv)
az storage blob list --account-name "$STORAGE_ACCOUNT" --container-name claims --account-key "$NEW_KEY1" -o table
```

After disabling Shared Key in Configuration, I tested the current key again (expected failure), then tested the Claims Reader's `--auth-mode login` access in a separate session. I restored the setting afterward and ran:

```bash
unset KEY1 KEY2 OLD_KEY1 NEW_KEY1
```

These are training commands. In production, plan for service dependencies and avoid passing secrets directly in command-line arguments where other users or process instrumentation may capture them.
