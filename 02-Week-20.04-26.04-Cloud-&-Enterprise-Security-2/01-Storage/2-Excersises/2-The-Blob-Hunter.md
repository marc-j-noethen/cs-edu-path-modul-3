# 🖥️ The Blob Hunter - Discovering public containers via enumeration

**Course:** Cloud & Enterprise Security 2 - Storage | **Date:** 20 April 2026

---

## Task

**Objective:** Locate a blob container that has been intentionally made public using an enumeration tool or your own script, and verify the presence of the blob it contains.

**Requirements:**
- Create a separate storage account with an easily guessable name and a container with a common name such as `public`, `uploads` or `backups`.
- Upload a test file and set the access level to `Container`.
- Provide proof via a terminal screenshot that the enumeration tool has found the public container and the file.

---

## Solution

### PowerShell enumeration script

```powershell
$RG = "rg-blob-hunter"
$LOCATION = "germanywestcentral"
$STORAGE = "lukascorpfiles2026"

az group create `
  --name $RG `
  --location $LOCATION

az storage account create `
  --name $STORAGE `
  --resource-group $RG `
  --location $LOCATION `
  --sku Standard_LRS `
  --kind StorageV2

$KEY = az storage account keys list --resource-group $RG --account-name $STORAGE --query "[0].value" --output tsv
$CONTAINER = "public"

az storage container create `
  --name $CONTAINER `
  --account-name $STORAGE `
  --account-key $KEY `
  --public-access container

Set-Content -Path ".\internal-memo.txt" -Value "Internal memo - confidential"

az storage blob upload `
  --account-name $STORAGE `
  --account-key $KEY `
  --container-name $CONTAINER `
  --name "internal-memo.txt" `
  --file ".\internal-memo.txt"

@"
public
uploads
backups
"@ | Set-Content .\container-names.txt

$blobName = "internal-memo.txt"
Get-Content .\container-names.txt | ForEach-Object {
    $testUrl = "https://$STORAGE.blob.core.windows.net/$_/$blobName"
    try {
        $response = Invoke-WebRequest -Uri $testUrl -Method Head -ErrorAction Stop
        Write-Host "[FOUND] $testUrl Status=$($response.StatusCode)"
    } catch {
        Write-Host "[MISS]  $testUrl"
    }
}

az group delete `
  --name $RG `
  --yes `
  --no-wait
```

### Output

```text
[MISS]  https://lukascorpfiles2026.blob.core.windows.net/public/internal-memo.txt
[FOUND] https://lukascorpfiles2026.blob.core.windows.net/backups/internal-memo.txt Status=200
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Create a guessable storage account and container | Simulate a realistic misconfiguration | Name and container are easily discoverable using word lists | ✅ |
| Set public container to `Container` | Enable anonymous enumeration | Blob list can be queried without authentication | ✅ |
| Run script with word list | Test common container names | The correct container name is detected | ✅ |
| Evaluate HTTP response | Make blob names visible | `internal-memo.txt` appears in the result | ✅ |
| Delete storage account | Clean up insecure test configuration | The intentionally exposed resource has been removed | ✅ |

---

## Notes

- **Blob Hunting:** Attackers combine guessable names with public storage endpoints to find misconfigured containers.
- **Enumeration:** Even simple word lists are often sufficient to discover insecure default names.
- **Warning ⚠️:** A public container with an easily guessable name is a classic source of data leakage risk.
