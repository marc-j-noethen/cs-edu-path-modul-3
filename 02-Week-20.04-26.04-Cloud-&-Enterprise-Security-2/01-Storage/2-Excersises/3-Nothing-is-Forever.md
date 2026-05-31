# 🖥️ Nothing is Forever - Upload a file privately and generate a temporary SAS URL

**Course:** Cloud & Enterprise Security 2 - Storage | **Date:** 20 April 2026

---

## Task

**Objective:** Write a Python CLI tool that uploads a local file to a private container and then outputs a read-only SAS URL valid for 5 minutes.

**Requirements:**
- Use a private container named `uploads`.
- Upload a local file specified via a command-line argument.
- Generate a SAS URL with `Read` permission only and a 5-minute expiry time.

---

## Solution

### Python script

```python
import os
import sys
from datetime import datetime, timedelta, timezone
from azure.storage.blob import BlobServiceClient, BlobSasPermissions, generate_blob_sas

CONTAINER_NAME = "uploads"

def main():
    if len(sys.argv) != 2:
        print("Usage: python upload_and_share.py <file_path>")
        sys.exit(1)

    file_path = sys.argv[1]
    file_name = os.path.basename (file_path)

    account_name = os.environ["AZURE_STORAGE_ACCOUNT_NAME"]
    account_key = os.environ["AZURE_STORAGE_ACCOUNT_KEY"]

    service_client = BlobServiceClient(
        account_url=f"https://{account_name}.blob.core.windows.net",
        credential=account_key,
    )

    blob_client = service_client.get_blob_client(
        container=CONTAINER_NAME,
        blob=file_name,
    )

    with open(file_path, "rb") as data:
        blob_client.upload_blob(data, overwrite=True)

    expiry = datetime.now(timezone.utc) + timedelta(minutes=5)

    sas_token = generate_blob_sas(
        account_name=account_name,
        container_name=CONTAINER_NAME,
        blob_name=file_name,
        account_key=account_key,
        permission=BlobSasPermissions(read=True),
        expiry=expiry,
    )

    sas_url = f"https://{account_name}.blob.core.windows.net/{CONTAINER_NAME}/{file_name}?{sas_token}"
    print(sas_url)

if __name__ == "__main__":
    main()
```

### Output

```text
https://mystorageaccount.blob.core.windows.net/uploads/internal-memo.txt?sv=...&sp=r&se=2026-04-28T12%3A35%3A00Z&...
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Read file argument | Specify target file | The script processes the passed local path | ✅ |
| Upload to `uploads` | Save file privately | The blob is stored in the private container | ✅ |
| `generate_blob_sas(...)` with `read=True` | Generate a time-limited share link | The SAS grants read-only access | ✅ |
| Expiry time `+ timedelta(minutes=5)` | Strictly limit access | The URL expires after 5 minutes | ✅ |
| Output the full URL | Enable sharing and browser testing | The SAS URL can be opened directly in the browser | ✅ |

---

## Notes

- **SAS:** A Shared Access Signature delegates time-limited and functionally restricted access to exactly one storage object.
- **Private Container:** The container remains private; only the generated SAS URL allows access to the blob.
- **Warning ⚠️:** Even a temporary SAS URL is a valid access token and should only be shared under controlled circumstances.

