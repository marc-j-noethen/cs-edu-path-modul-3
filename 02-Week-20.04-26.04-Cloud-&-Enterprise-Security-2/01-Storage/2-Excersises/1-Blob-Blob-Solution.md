# 🖥️ Blob Blob - Upload a public blob and access it via the web

**Course:** Cloud & Enterprise Security 2 - Storage | **Date:** 20 April 2026

---

## Task

**Objective:** Upload a blob to Azure Storage and verify that it can be accessed anonymously via a web browser.

**Prerequisites:**
- Completion of the Microsoft Learn lab `describe-azure-storage-services/5-exercise-create-storage-blob`.
- Upload a file to a blob container with the appropriate public access configuration.
- Submit a screenshot showing successful web access to the blob.

---

## Solution

### Portal steps

```text
1. Complete the Microsoft Learn lab on creating a storage blob.
2. Create a new storage account and a blob container.
3. Upload the desired file to the container.
4. Configure the container or blob to allow anonymous read access.
5. Copy the blob URL.
6. Open the URL in a browser and verify that the file loads.
7. Take a screenshot of the successful web access.
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Create blob container | Prepare storage destination | The container is available in the storage account | ✅ |
| Upload file | Provide test content | The blob is visible in the container | ✅ |
| Configure public access | Enable anonymous web access | The blob is accessible without logging in | ✅ |
| Open blob URL in browser | Test access | The file is successfully displayed or downloaded | ✅ |

---

## Notes

- **Blob Storage:** Azure Blob Storage is designed for unstructured data such as files, images and documents.
- **Anonymous access:** Public access should only be enabled for content that has been deliberately shared.
- **Warning ⚠️:** Publicly shared containers or blobs may expose sensitive data if they are configured incorrectly.

