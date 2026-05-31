# 🖥️ Building & Testing the Vault - Creating an Azure Container Registry and testing the push

**Course:** CES DevOps 2 - Image Security & Azure Registry | **Date:** 06 May 2026

---

## Task

**Objective:** Create an Azure Container Registry, verify the login, and successfully push a test image to the private repository.

**Requirements:**
- Create a new ACR in the Basic tier in the Azure portal.
- Enable the admin user or, alternatively, generate a token.
- Pull a public image locally, retag it appropriately, and push it to the new registry.
- Check in the portal that the repository and its tag are visible.

---

## Solution

### Docker and Azure commands

```bash
acr_name=<your-acr-name>
login_server=<your-acr-name>.azurecr.io

az acr login --name $acr_name

docker pull python:3.11-slim
docker tag python:3.11-slim $login_server/python-slim:lab
docker push $login_server/python-slim:lab
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| Create ACR in the portal | Deploy private registry | An empty Azure Container Registry is ready for use. | ✅ |
| `az acr login` | Test authentication against the registry | The local Docker client is permitted to access the ACR. | ✅ |
| `docker tag` + `docker push` | Verify first successful artefact flow | The test image is published as a new repository/tag in the ACR. | ✅ |

---

## Notes

- **ACR:** Azure Container Registry is Azure’s managed container registry for images and artefacts.
- **Login Server:** The push destination name follows the pattern `<acr-name>.azurecr.io/<repository>:<tag>`.
- **Basic SKU:** The affordable standard configuration is perfectly adequate for this learning exercise.

