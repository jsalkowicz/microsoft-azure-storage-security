# Lab 2 — Storage Account Keys (Portal Ready)

## Objective

Practice the REAL Azure lifecycle:

**identify dependency -> use secondary key -> rotate primary key -> prove old key fails -> migrate away from Shared Key**

Use a disposable training Storage account only.

## Step 1 — Create/reuse a training Storage account

Create:

- private container `claims`
- private container `humanresources`

Upload the included synthetic files.

## Step 2 — Locate the two real account keys

Azure Portal:

**Storage account -> Security + networking -> Access keys**

Azure shows:

- key1
- key2

### SECURITY RULE

Do NOT paste either real key into ChatGPT, screenshots, source control, email, or permanent notes.

## Step 3 — Use Key 1 from Cloud Shell without writing it into a file

In Cloud Shell, set the key as a temporary shell variable.

Replace the placeholder yourself:

```bash
export STORAGE_ACCOUNT="<your-account-name>"
export KEY1="<paste-key1-here>"
```

List Claims using Shared Key:

```bash
az storage blob list \
  --account-name "$STORAGE_ACCOUNT" \
  --container-name claims \
  --account-key "$KEY1" \
  --output table
```

Then list HR:

```bash
az storage blob list \
  --account-name "$STORAGE_ACCOUNT" \
  --container-name humanresources \
  --account-key "$KEY1" \
  --output table
```

Notice the important lesson:

> The same account key is not naturally limited to only the Claims container.

## Step 4 — Switch the test client to Key 2

In the Portal copy Key 2 into a temporary Cloud Shell variable:

```bash
export KEY2="<paste-key2-here>"
```

Test Claims:

```bash
az storage blob list \
  --account-name "$STORAGE_ACCOUNT" \
  --container-name claims \
  --account-key "$KEY2" \
  --output table
```

If it succeeds, your test client can operate using Key 2.

## Step 5 — Rotate Key 1

Azure Portal:

**Security + networking -> Access keys**

Regenerate **key1**.

Do NOT regenerate both keys together.

## Step 6 — Prove the old Key 1 no longer works

Your Cloud Shell variable still contains the OLD Key 1 value.

Run:

```bash
az storage blob list \
  --account-name "$STORAGE_ACCOUNT" \
  --container-name claims \
  --account-key "$KEY1" \
  --output table
```

Expected:

**authentication failure**

Now use Key 2 again.

Expected:

**success**

This proves why Azure provides two keys.

## Step 7 — Refresh your Key 1 value

Copy the NEW Key 1 value into:

```bash
export NEW_KEY1="<new-key1-value>"
```

Test it.

## Step 8 — Understand production rotation

Real production sequence:

```text
Find every client using Key 1
-> move those clients to Key 2
-> regenerate Key 1
-> test
-> move clients to new Key 1 if desired
-> regenerate Key 2
-> test again
```

Rotation is both:

- a security action
- a production change

## Step 9 — Compare with Entra ID

Now run an Entra-authenticated command if your identity has an appropriate Blob data role:

```bash
az storage blob list \
  --account-name "$STORAGE_ACCOUNT" \
  --container-name claims \
  --auth-mode login \
  --output table
```

This does NOT send an account key.

## Step 10 — Optional: Disable Shared Key

ONLY after you are finished with the key tests.

Portal:

**Settings -> Configuration -> Allow storage account key access -> Disabled**

Save.

Now try Key 2 again.

Expected:

**Shared Key request rejected**

If you need the account for the SAS lab, re-enable Shared Key only if the specific SAS exercise requires a key-signed SAS. User delegation SAS can use Entra credentials.

## Final memory map

```text
FIND -> SWITCH -> ROTATE -> TEST -> MIGRATE -> DISABLE
```

Best long-term direction for supported Azure workloads:

```text
Entra ID + managed identity + scoped RBAC
```
