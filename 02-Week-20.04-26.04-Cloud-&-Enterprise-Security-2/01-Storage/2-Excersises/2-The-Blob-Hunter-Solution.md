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

### Custom enumeration script

```python
import requests

account = "<storageaccountname>"
containers = ["public", "uploads", "backups", "files", "documents"]

for container in containers:
    url = f"https://{account}.blob.core.windows.net/{container}?restype=container&comp=list"
    response = requests.get(url, timeout=5)

    if response.status_code == 200:
        print(f"[+] Public container found: {container}")
        print(response.text)
    else:
        print(f"[-] No match: {container} ({response.status_code})")
```

### Output

```text
[-] No match: public (404)
[+] Public container found: backups
<?xml version="1.0" encoding="utf-8"?>
<EnumerationResults>
  ...
  <Name>internal-memo.txt</Name>
  ...
</EnumerationResults>
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Create a guessable storage account and container | Simulate a realistic misconfiguration | Name and container are easily discoverable using word lists | ✅ |
| Set public container to `Container` | Enable anonymous enumeration | Blob list can be queried without authentication | ✅ |
| Run script with word list | Test common container names | The correct container name is detected | ✅ |
| Evaluate XML response | Make blob names visible | `internal-memo.txt` appears in the result | ✅ |
| Delete storage account | Clean up insecure test configuration | The intentionally exposed resource has been removed | ✅ |

---

## Notes

- **Blob Hunting:** Attackers combine guessable names with public storage endpoints to find misconfigured containers.
- **Enumeration:** Even simple word lists are often sufficient to discover insecure default names.
- **Warning ⚠️:** A public container with an easily guessable name is a classic source of data leakage risk.

