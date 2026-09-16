# Lab 1 — Azure Storage Security Features (Portal Ready)

## Objective

Practice:

- encryption at rest
- encryption in transit
- CORS
- Entra ID + RBAC
- storage access logging

## Step 1 — Create the training Storage account

In Azure Portal:

1. Search **Storage accounts**.
2. Select **Create**.
3. Create a new resource group such as `rg-storage-security-lab`.
4. Choose a globally unique Storage account name.
5. Keep **Require secure transfer** enabled.
6. Keep **Allow Blob anonymous access** disabled.
7. Create the account.

Write down only the **Storage account name**. Do not copy any keys into your notes.

## Step 2 — Create containers and upload the included files

In the Storage account:

**Data storage -> Containers**

Create PRIVATE containers:

- `claims`
- `humanresources`
- `web-assets`

Upload:

- `synthetic-data/claims/*` -> `claims`
- `synthetic-data/humanresources/*` -> `humanresources`
- `synthetic-data/web-assets/banner.txt` -> `web-assets`

## Step 3 — Verify encryption at rest

Open the Storage account's **Encryption** page.

Observe that Azure Storage encryption is enabled.

What this proves:

> Azure encrypts the stored copy of your Blob data.

It does NOT prove that every identity is authorized to read it.

## Step 4 — Verify encryption in transit

Go to:

**Settings -> Configuration**

Confirm:

**Secure transfer required = Enabled**

Optional Cloud Shell check:

```bash
az storage account show \
  --resource-group <RESOURCE_GROUP> \
  --name <STORAGE_ACCOUNT> \
  --query enableHttpsTrafficOnly
```

Expected:

```text
true
```

## Step 5 — Configure CORS for the web-assets container service

In the Storage account go to:

**Settings -> Resource sharing (CORS)**

On **Blob service**, add:

- Allowed origins: `http://localhost:8000`
- Allowed methods: `GET`
- Allowed headers: `*`
- Exposed headers: `*`
- Max age: `200`

Save.

CORS is NOT authentication. The blob still needs valid authorization.

### CORS test

On your computer, from this lab folder:

```bash
python -m http.server 8000
```

Open:

```text
http://localhost:8000/cors_test.html
```

Create a short-lived READ-only SAS URL for `web-assets/banner.txt`, paste the URL into the page, and select **GET blob**.

Then change/remove the CORS origin and retest.

## Step 6 — Practice Entra ID + RBAC

Business requirement:

> A Claims identity should read Claims data, but should not automatically receive HR access.

At the `claims` container:

**Access control (IAM) -> Add role assignment**

Choose:

**Storage Blob Data Reader**

Assign it to the test identity you are using.

Use Cloud Shell with Entra authentication:

```bash
az storage blob list \
  --account-name <STORAGE_ACCOUNT> \
  --container-name claims \
  --auth-mode login \
  --output table
```

If the identity has the data role, Claims should work.

Try HR:

```bash
az storage blob list \
  --account-name <STORAGE_ACCOUNT> \
  --container-name humanresources \
  --auth-mode login \
  --output table
```

### Important

If your signed-in account already has a broader Blob data role at the Storage account, resource group, or subscription level, the HR test may also succeed.

That does NOT mean container-scoped RBAC failed. It means your identity has another broader permission.

For a clean allow/deny test, use a separate test identity that has only the role you assign for this lab.

## Step 7 — Enable modern storage logging

For a live Azure exercise, use Azure Monitor diagnostic settings rather than relying only on the synthetic log.

In the Storage account, locate the Blob service diagnostic settings/monitoring options available in your tenant and send Blob read/write/delete logs to a Log Analytics workspace if your subscription permits it.

Generate some activity, then query the resulting storage logs.

If your tenant/subscription does not allow you to configure a Log Analytics workspace, use:

`evidence/synthetic_storage_access_log.csv`

as the investigation portion of the lab.

## Final memory map

```text
AT REST -> IN TRANSIT -> BROWSER -> ACCESS -> AUDIT
SSE     -> HTTPS      -> CORS    -> RBAC   -> LOGS
```
