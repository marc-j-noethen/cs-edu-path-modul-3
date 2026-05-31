# 🖥️ The Secret Masker - Using GitHub Secrets securely and proving masking

**Course:** CES DevOps 1 - GitHub Actions & CI/CD | **Date:** 28 April 2026

---

## Task

**Objective:** Use a repository secret in a workflow without the plaintext value being visible in the GitHub Actions log.

**Requirements:**
- Create a repository secret `MY_SENSITIVE_TOKEN` under `Settings -> Secrets and variables -> Actions`.
- Create a manually triggerable workflow `secret_test.yml`.
- Reference the secret in the run and start the workflow via `workflow_dispatch`.
- Check the log to ensure that GitHub masks the value and does not display it in plain text.

---

## Solution

### Workflow file

```yaml
name: secret-test

on:
  workflow_dispatch:

jobs:
  mask-secret:
    runs-on: ubuntu-latest
    steps:
      - name: Print token safely
        run: echo "Token=${{ secrets.MY_SENSITIVE_TOKEN }}"
```

### Procedure

```text
1. Create the secret `MY_SENSITIVE_TOKEN` in the repository.
2. Commit and push `secret_test.yml`.
3. Manually start the workflow in the "Actions" tab.
4. Check the log to ensure that GitHub masks the actual secret value as `***`.
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| Repository Secret | Separate sensitive values from the code | The token is not in the repository, but in the GitHub Secret Store. | ✅ |
| `workflow_dispatch` | Manually execute a controlled test | The workflow starts at the click of a button, without requiring a new commit. | ✅ |
| Masking in the log | Prevent leakage | GitHub visibly replaces the plaintext value with `***`. | ✅ |

---

## Notes

- **GitHub Secrets:** The secret value is only injected into the runner at runtime.
- **Masking:** The display `***` is the expected, actual behaviour in the job log.
- **Security rule:** Even with masked secrets, you should avoid intentionally printing them in real workflows because redaction is helpful but not a perfect security boundary.

