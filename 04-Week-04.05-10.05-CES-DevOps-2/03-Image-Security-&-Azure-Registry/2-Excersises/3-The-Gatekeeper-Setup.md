# 🖥️ The Gatekeeper Setup - Preparing the GitHub repository, secrets and ACR Gatekeeper

**Course:** CES DevOps 2 - Image Security & Azure Registry | **Date:** 06 May 2026

---

## Task

**Objective:** Prepare a GitHub repository so that a pipeline builds images, scans them with Trivy, and only pushes them to the Azure Container Registry if successful.

**Requirements:**
- Create a new public GitHub repository.
- Save the secrets `ACR_LOGIN_SERVER`, `ACR_USERNAME` and `ACR_PASSWORD` as repository secrets.
- Commit the workflow file `.github/workflows/gatekeeper.yml` containing the build, scan and push logic.
- Verify in the repository that the secrets are stored correctly.

---

## Solution

### Workflow file

```yaml
name: Security Gatekeeper Pipeline

on:
  push:
    branches: ["main", "vulnerable"]
  workflow_dispatch:

jobs:
  build-scan-push:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Login to ACR
        uses: docker/login-action@v3
        with:
          registry: ${{ secrets.ACR_LOGIN_SERVER }}
          username: ${{ secrets.ACR_USERNAME }}
          password: ${{ secrets.ACR_PASSWORD }}

      - name: Build image
        run: docker build -t ${{ secrets.ACR_LOGIN_SERVER }}/vulnerable-app:${{ github.sha }} .

      - name: Run Trivy security scan
        uses: aquasecurity/trivy-action@0.28.0
        with:
          image-ref: ${{ secrets.ACR_LOGIN_SERVER }}/vulnerable-app:${{ github.sha }}
          format: table
          severity: CRITICAL
          ignore-unfixed: true
          exit-code: '1'

      - name: Push image to ACR
        if: success()
        run: |
          docker push ${{ secrets.ACR_LOGIN_SERVER }}/vulnerable-app:${{ github.sha }}
          echo "Success! Image pushed to Azure."
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| Repository Secrets | Keep ACR credentials out of the code | The pipeline only receives registry credentials at runtime. | ✅ |
| Trivy before the push | Stop insecure images early | Only successful scans reach the push step at all. | ✅ |
| `if: success()` | Tightly couple the push to the scan | Nothing is uploaded to ACR if the scan fails. | ✅ |

---

## Notes

- **Workflow correction:** The `severity` and `ignore-unfixed` inputs must be in English and named exactly as such in proper YAML.
- **ACR secrets:** Only the names of the secrets should be visible in the screenshot, not their values.
- **Gatekeeper principle:** The scanner is intentionally positioned between build and publish.

