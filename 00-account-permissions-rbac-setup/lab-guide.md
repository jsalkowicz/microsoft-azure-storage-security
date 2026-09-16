# LAB 0 — Prepare Your Azure Account for the Storage Security Labs

## What this lab does

This lab gets your Azure account ready for the other hands-on labs.

By the end, you should know:

1. Do I have an Azure subscription I can use?
2. Can I create a resource group?
3. Can I create a Storage account?
4. Can I assign Azure RBAC roles?
5. Can I create a separate Microsoft Entra test user?
6. Can I prove that the test user has access to Claims but not HR?

This is a **training environment only**. Do not use production resources or real customer data.

---

# The simple mental model

For the later labs, you need two identities:

```text
YOUR ADMIN / LAB-BUILDER ACCOUNT
        |
        | creates resources and assigns access
        v
AZURE TRAINING ENVIRONMENT


SEPARATE TEST IDENTITY
        |
        | receives ONLY the permission being tested
        v
CLAIMS ALLOWED / HR DENIED
```

Why use a separate identity?

Because your normal account may already have broad permissions.

If your account is already an Owner, Contributor, or Blob Data Owner, then:

```text
"Claims allowed"
```

might work, but:

```text
"HR denied"
```

might also work accidentally because your account already has permission.

A clean test identity lets you prove **least privilege**.

---

# PART 1 — Confirm you have an Azure subscription

## Portal method

1. Sign in to:

   `https://portal.azure.com`

2. In the top search bar, search for:

   `Subscriptions`

3. Open **Subscriptions**.

You should see at least one subscription with a usable status.

Write down:

```text
Subscription name:
Subscription ID:
Tenant / directory:
```

Do NOT send the subscription ID to anyone unless you have a specific reason.

## Cloud Shell check

Open **Cloud Shell** in the Azure portal.

Run:

```bash
az account show --output table
```

Then:

```bash
az account list --output table
```

You should see your active subscription.

If you do NOT have a subscription, stop here and create or activate an Azure subscription before continuing.

---

# PART 2 — Create a dedicated training resource group

A resource group is simply a folder-like boundary for Azure resources.

Create one just for these labs.

## Portal

1. Search for **Resource groups**.
2. Select **Create**.
3. Choose your subscription.
4. Resource group name:

```text
rg-northstar-storage-labs
```

5. Pick a region near you.
6. Select **Review + create**.
7. Select **Create**.

## Why this matters

When the labs are finished, you can delete the entire resource group and clean up everything inside it.

---

# PART 3 — Prove you can create resources

Inside:

```text
rg-northstar-storage-labs
```

create a test Storage account.

Suggested name pattern:

```text
northstarlab<randomnumbers>
```

Storage account names must be globally unique.

For example:

```text
northstarlab82741
```

During creation:

- Use the training resource group.
- Keep **Secure transfer required** enabled.
- Keep public Blob access disabled unless a later lab specifically changes something.
- Standard performance is fine for these exercises.
- Use inexpensive/default redundancy for training unless your subscription requires something different.

After deployment, open the Storage account.

If this succeeds, you have confirmed:

```text
Subscription works
+
Resource group creation works
+
Storage account creation works
```

---

# PART 4 — Check whether you can assign Azure RBAC roles

This is important.

Open:

```text
Resource groups
-> rg-northstar-storage-labs
-> Access control (IAM)
```

Open:

```text
Role assignments
```

Then look for:

```text
Add
-> Add role assignment
```

## Result A — Button is available

Good.

You likely have permission to create Azure role assignments at this scope.

Azure role assignments require the ability to perform:

```text
Microsoft.Authorization/roleAssignments/write
```

Roles such as these can include that capability:

```text
Role Based Access Control Administrator
User Access Administrator
Owner
```

Your tenant may use different/custom permissions.

## Result B — Add role assignment is disabled

That means your current identity does not have permission to assign Azure roles at this scope.

You can still do portions of the Storage labs, but the clean RBAC allow/deny exercises will require someone with permission to grant the role.

Record your result in:

`LAB0_RESULTS.md`

---

# PART 5 — Check your own access

At the training resource group:

```text
Access control (IAM)
```

Review your role assignments.

You are trying to answer:

```text
Why can my account do what it can do?
```

Possible examples:

```text
Owner
Contributor
Storage Blob Data Owner
User Access Administrator
```

The assignment might come from:

```text
Subscription
Resource group
Specific resource
Group membership
```

Remember:

> A role inherited from a broader scope also applies to narrower resources.

Example:

```text
Owner at Subscription
        |
        v
Resource Group
        |
        v
Storage Account
        |
        v
Containers
```

That is why your normal account might be a poor identity for a clean "HR denied" test.

---

# PART 6 — Create a separate Microsoft Entra test identity

## Preferred method

Create a dedicated synthetic training user such as:

```text
Display name:
Northstar Claims Reader

Username:
northstar.claimsreader@<your-tenant-domain>
```

Do NOT give this test user any Entra admin role.

### Portal path

In Azure Portal or Microsoft Entra admin center:

```text
Microsoft Entra ID
-> Users
-> New user
-> Create new user
```

Create the account.

A tenant typically requires a role such as **User Administrator** to create a new user.

Record the temporary password securely.

Sign in once as the test user if your tenant requires a password change.

### IMPORTANT

This test identity should start with no special Azure resource access.

The entire point is:

```text
NO ACCESS
-> ASSIGN ONE ROLE
-> TEST EXACTLY WHAT CHANGED
```

---

# PART 7 — If you cannot create a test user

Do NOT give up.

Use this decision tree.

## Option 1 — You have permission to invite a guest

If your tenant allows it and you have a role such as Guest Inviter/User Administrator, you can invite a second email account you control as a guest.

Use the guest only for the lab.

## Option 2 — Your organization controls Entra user creation

Ask the tenant administrator for a temporary test identity with:

```text
No admin roles
No Azure resource roles by default
```

Then you assign only the lab-specific Azure role.

## Option 3 — You cannot get a second identity

You can still perform:

- Storage account creation
- encryption settings
- secure transfer
- CORS
- Shared Key
- SAS
- storage logging

For RBAC:

- study the assignment
- inspect role scopes
- use your own account for positive tests

But you should NOT treat a failed "HR denied" test as valid if your normal account has broad inherited permissions.

The clean negative test must wait until you have a restricted identity.

---

# PART 8 — Build the Claims vs HR test environment

In the Storage account create two PRIVATE containers:

```text
claims
hr
```

Upload one synthetic file into each container when you run the Storage Security Features lab.

The intended authorization model is:

```text
Northstar Claims Reader
        |
        | Storage Blob Data Reader
        v
CLAIMS CONTAINER
```

There should be NO Blob data role for this test user on:

```text
hr
```

---

# PART 9 — Assign the Claims-only role

Open:

```text
Storage account
-> Data storage
-> Containers
-> claims
-> Access control (IAM)
```

Select:

```text
Add
-> Add role assignment
```

Choose:

```text
Storage Blob Data Reader
```

Member:

```text
Northstar Claims Reader
```

Scope:

```text
claims container
```

Complete the assignment.

Allow a few minutes for the RBAC assignment to propagate.

## Important

Do NOT assign the same test identity a Blob data role at:

```text
Subscription
Resource group
Storage account
```

If you do, the broader assignment could also give it HR access.

---

# PART 10 — Prove Claims allowed / HR denied

Sign in as the TEST identity, not your admin identity.

A private/incognito browser profile is useful so the sessions do not get mixed.

You can use Cloud Shell if the test identity is allowed to launch it and your environment supports it.

Set:

```bash
STORAGE_ACCOUNT="<your-storage-account-name>"
```

Test Claims:

```bash
az storage blob list \
  --account-name "$STORAGE_ACCOUNT" \
  --container-name claims \
  --auth-mode login \
  --output table
```

Expected:

```text
SUCCESS
```

Now test HR:

```bash
az storage blob list \
  --account-name "$STORAGE_ACCOUNT" \
  --container-name humanresources \
  --auth-mode login \
  --output table
```

Expected:

```text
AUTHORIZATION FAILURE
```

That proves:

```text
Same identity
Same Storage account
Different RBAC scope
Different result
```

That is the clean least-privilege test.

---

# PART 11 — Troubleshoot if HR unexpectedly works

If the test user can read HR, do NOT assume RBAC is broken.

Check for broader permissions.

Look at role assignments for the test identity at:

```text
Subscription
Resource group
Storage account
humanresources container
```

Possible cause:

```text
Storage Blob Data Reader
at Storage account scope
```

That means:

```text
claims = allowed
humanresources = also allowed
```

because the role applies to the whole Storage account.

Another possible cause:

```text
test user belongs to a group
```

and that group has broader Blob permissions.

The lesson:

> Effective access is the combination of direct + inherited + group-based access.

---

# PART 12 — Troubleshoot if Claims is denied

Check:

1. Did you assign **Storage Blob Data Reader**, not just Reader?
2. Was the role assigned to the correct test identity?
3. Is the scope the Claims container?
4. Has RBAC had time to propagate?
5. Are you actually signed in as the test identity?
6. Are you testing with:

```text
--auth-mode login
```

instead of a storage account key?
7. Is there a Conditional Access or tenant policy affecting the sign-in?

Do NOT solve the problem by immediately giving the user Owner or Contributor.

Fix the actual missing permission.

---

# PART 13 — Know the role difference

These are different:

```text
Reader
```

means roughly:

> Can view Azure resource configuration.

It does NOT automatically mean:

> Can read Blob contents.

For Blob contents, the relevant role is something like:

```text
Storage Blob Data Reader
```

This distinction is extremely important in Azure.

Think:

```text
MANAGEMENT PLANE
vs
DATA PLANE
```

---

# PART 14 — Prove you are ready for the next labs

Before starting Lab 1, you should be able to check these boxes:

```text
[ ] I have an active Azure subscription.
[ ] I created rg-northstar-storage-labs.
[ ] I created a disposable Storage account.
[ ] I know whether I can add Azure role assignments.
[ ] I know what Azure roles my normal account already has.
[ ] I created or obtained a separate restricted test identity.
[ ] I created claims and humanresources containers.
[ ] Test identity has Storage Blob Data Reader ONLY on claims.
[ ] Claims read works.
[ ] HR read fails.
```

If all are checked:

**Your Azure training environment is ready.**

---

# PART 15 — Cleanup

When you have completed ALL of the Azure labs:

```text
Azure Portal
-> Resource groups
-> rg-northstar-storage-labs
-> Delete resource group
```

Review the resources first.

Delete only the training resource group you created for these exercises.

If you created a dedicated Entra test user and no longer need it, remove it separately through your normal tenant administration process.

---

# Memory map

The prerequisite lab is:

```text
SUBSCRIPTION
     |
RESOURCE GROUP
     |
STORAGE ACCOUNT
     |
ADMIN IDENTITY
     |
TEST IDENTITY
     |
RBAC ROLE
     |
SCOPE
     |
ALLOW + DENY TEST
```

The sentence to remember:

> **My admin account builds the lab. My restricted test account proves least privilege.**
