# Cloud Shell command record — Lab 03

Commands below represent the **actual CLI portions** of the live lab. SAS generation itself was done in the Azure portal for both tests. Replace no placeholders with real secrets in a GitHub commit. Use Bash in the authorized training shell and run only as needed; the original credentials have expired.

## Load a fresh SAS token privately

Copy the **token only**, not a full URL. Do not print it or enable `set -x` / `--debug`:

```bash
read -r -s -p "Paste upload SAS token, then press Enter: " UPLOAD_SAS; echo
```

## First synthetic upload (actually run)

Transfer the local `claim_photo_CL-6101.txt` with Cloud Shell → Manage files → Upload, then:

```bash
az storage blob upload \
  --account-name northstarlab48217 \
  --container-name claims-upload \
  --name claim_photo_CL-6101.txt \
  --file /home/john/claim_photo_CL-6101.txt \
  --sas-token "$UPLOAD_SAS" \
  --overwrite false \
  --output none && echo "SAS upload succeeded"
```

### Permission denials (actually run with that same token)

```bash
az storage blob download \
  --account-name northstarlab48217 \
  --container-name claims-upload \
  --name claim_photo_CL-6101.txt \
  --file /home/john/sas-read-test.txt \
  --sas-token "$UPLOAD_SAS" --output none

az storage blob list \
  --account-name northstarlab48217 \
  --container-name claims-upload \
  --sas-token "$UPLOAD_SAS" --output table

az storage blob delete \
  --account-name northstarlab48217 \
  --container-name claims-upload \
  --name claim_photo_CL-6101.txt \
  --sas-token "$UPLOAD_SAS" --output none
```

These three commands returned permission errors. The file was still present in the portal.

## Second synthetic upload, after enabling Storage Write logs (actually run)

A **new** SAS token was created in the portal and loaded privately again. Transfer the provided second synthetic file to Cloud Shell before running:

```bash
az storage blob upload \
  --account-name northstarlab48217 \
  --container-name claims-upload \
  --name customer_submission_note_CL-6101.txt \
  --file /home/john/customer_submission_note_CL-6101.txt \
  --sas-token "$UPLOAD_SAS" \
  --overwrite false \
  --output none && echo "Second SAS upload succeeded"
```

The command succeeded; the new audit record showed `PutBlob`, `SAS`, `201 Success`.

```bash
unset UPLOAD_SAS
```

**Note:** The read-only blob SAS was tested by opening and modifying a temporary URL in a private browser tab. Its token was not used in these Cloud Shell commands. Shell variables protect against accidental console display, but they are not an enterprise secret store; use a controlled workflow for production credentials.
