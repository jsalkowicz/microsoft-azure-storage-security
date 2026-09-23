# SAS design and incident notes

## What I used in this lab

- **Repair-partner access:** account-key-signed, **blob-scoped service SAS** with Read permission, HTTPS-only, short expiry. The approved blob was accessible; a different blob and the expired original link were denied.
- **Customer submission:** separate account-key-signed, **container-scoped service SAS** on a private, dedicated upload container. I selected Create + Write; Read, List and Delete were not granted. The upload worked and the restricted operations were denied.
- The strings `claims-read` and `claims-upload` have different roles here: `claims-read` names a local synthetic-data folder, while `claims-upload` is an actual Azure container.

## What I would change for a real system

A container-scoped Create + Write token is wider than a one-file submission capability: it is not tied to a specific blob path, and Write can permit changing an existing blob. I would issue a one-blob SAS for a server-selected, unpredictable filename with an overwrite-safe upload design, short lifetime, HTTPS, and the minimum permissions required by the actual client API. I would keep submissions separate from approved claim and HR material, log writes and review who can generate SAS in the first place.

Where an appropriately authorized Entra identity is available, I would evaluate a **user delegation SAS** for Blob storage rather than routinely signing with account keys. That was **not tested** in this run. A service/account SAS depends on Shared Key authorization; a user delegation SAS uses a different signing mechanism. The generic `AuthenticationType = SAS` log value by itself does not identify the signing method or unique SAS credential.

## If a signed URL were exposed

A SAS URL is a bearer credential. I would remove it from public locations, identify the signed resource, permissions, type, and expiration, and review logs for observed access. A published link is not proof that data was read. I would assess revocation mechanisms and affected dependencies before rotating a storage account key, which can invalidate other credentials. This is an incident-response *design consideration*, not an incident that occurred in this lab.

References: [SAS overview](https://learn.microsoft.com/en-us/azure/storage/common/storage-sas-overview), [Prevent Shared Key authorization](https://learn.microsoft.com/en-us/azure/storage/common/shared-key-authorization-prevent).
