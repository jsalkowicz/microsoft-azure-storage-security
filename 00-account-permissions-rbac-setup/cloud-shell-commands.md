# LAB 0 — Cloud Shell Command Sheet

## Confirm subscription

```bash
az account show --output table
az account list --output table
```

## Set variables

```bash
RESOURCE_GROUP="rg-northstar-storage-labs"
STORAGE_ACCOUNT="<your-storage-account-name>"
```

## Confirm resource group

```bash
az group show \
  --name "$RESOURCE_GROUP" \
  --output table
```

## Check Storage account

```bash
az storage account show \
  --resource-group "$RESOURCE_GROUP" \
  --name "$STORAGE_ACCOUNT" \
  --output table
```

## Test Claims using the signed-in Entra identity

```bash
az storage blob list \
  --account-name "$STORAGE_ACCOUNT" \
  --container-name claims \
  --auth-mode login \
  --output table
```

## Test HR using the signed-in Entra identity

```bash
az storage blob list \
  --account-name "$STORAGE_ACCOUNT" \
  --container-name humanresources \
  --auth-mode login \
  --output table
```

For the restricted Claims test identity:

Expected:

```text
claims = success
humanresources = authorization failure
```

Never paste real storage keys or SAS tokens into this file.
