# Lab 3 — Shared Access Signatures (SAS) (Portal + Cloud Shell Ready)

## Objective

Practice:

- short-lived delegated access
- permissions
- resource scope
- expiration
- user delegation SAS
- why Account SAS is broader

Core question:

**WHAT can they do? WHERE can they do it? HOW LONG can they do it?**

## Step 1 — Create containers

In the training Storage account create private containers:

- `claims-upload`
- `claims-read`

Upload the included synthetic files.

## Step 2 — Create a short-lived read SAS in the Portal

For `claims-read/claim_summary_CL-6201.txt`, use the Blob's SAS generation experience available in the Azure Portal.

Choose:

- Read only
- HTTPS only if available
- very short expiration, such as 15–30 minutes

Copy the generated Blob SAS URL.

### SECURITY RULE

Treat the real SAS URL like a password.

Do NOT paste it into ChatGPT or save it in source control.

Open the URL in a private/incognito browser.

Expected:

**the blob is readable while the SAS is valid**

Try again after expiration.

Expected:

**access fails**

## Step 3 — Generate a user delegation SAS in Azure Cloud Shell

Microsoft recommends user delegation SAS when possible because it is authorized with Entra credentials rather than an account key.

Your signed-in identity needs the required Blob data permissions and permission to request a user delegation key.

In Cloud Shell:

```bash
export STORAGE_ACCOUNT="<your-account-name>"
EXPIRY=$(date -u -d "30 minutes" '+%Y-%m-%dT%H:%MZ')
```

Generate a READ-only user delegation SAS for the claim summary:

```bash
az storage blob generate-sas \
  --account-name "$STORAGE_ACCOUNT" \
  --container-name claims-read \
  --name claim_summary_CL-6201.txt \
  --permissions r \
  --expiry "$EXPIRY" \
  --auth-mode login \
  --as-user \
  --full-uri
```

The command returns a SAS URI.

Do not paste that URI into ChatGPT.

Open it in a private browser and test it.

## Step 4 — Test the minimum-permission principle

Your repair partner only needs to READ one approved claim summary.

Correct design:

```text
Permission: Read
Resource: One blob
Time: Short
```

Do NOT give:

```text
Write
Delete
List
Long expiration
```

unless the business requirement actually needs them.

## Step 5 — Customer upload scenario

Business need:

> A customer needs to upload a claim photo.

Design the SAS so the customer has only the upload permissions needed for the upload location and only for a short period.

The important exercise is not memorizing permission letters.

It is choosing:

```text
minimum permission
minimum scope
minimum time
```

## Step 6 — Understand the three SAS types

### User delegation SAS

- backed by Microsoft Entra credentials
- preferred when supported

### Service SAS

- scoped to a specific Azure Storage service/resource
- signed with an account key

### Account SAS

- can grant broader storage capabilities
- signed with an account key
- larger possible blast radius

## Step 7 — Compare SAS with managed identity

Use **managed identity** when an Azure workload needs ongoing identity-based access.

Example:

```text
Claims App -> Blob Storage
```

Use **SAS** when a client needs temporary delegated access.

Example:

```text
Customer -> upload accident photo for 20 minutes
```

## Step 8 — Incident exercise

Assume a SAS URL is accidentally posted publicly.

Determine:

1. What permission does it grant?
2. What resource does it reach?
3. When does it expire?
4. Was it used?
5. How can you invalidate/revoke the access?
6. What logs show requests?

Do not assume exposure proves data access. Investigate the evidence.

## Final memory map

```text
SAS = TEMPORARY PERMISSION SLIP

WHAT?
WHERE?
HOW LONG?
```

Preferred when possible:

```text
User delegation SAS -> Entra-backed
```
