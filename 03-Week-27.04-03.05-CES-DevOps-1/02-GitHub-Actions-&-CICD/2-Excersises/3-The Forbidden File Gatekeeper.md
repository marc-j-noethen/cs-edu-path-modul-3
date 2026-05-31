# 🖥️ The Forbidden File Gatekeeper - Blocking forbidden files via the pipeline

**Course:** CES DevOps 1 - GitHub Actions & CI/CD | **Date:** 28 April 2026

---

## Task

**Objective:** Build a security barrier that automatically causes a push to fail as soon as a forbidden file appears in the repository.

**Requirements:**
- Create a workflow named `security_gate.yml` in the `actions-test` repository.
- The workflow should run on every push and check for `passwords.txt`.
- If the file is absent, the workflow must remain green; if the file is present, it must turn red.
- The log must clearly show why the build was stopped.

---

## Solution

### Workflow file

```yaml
name: security-gate

on:
  push:

jobs:
  block-forbidden-files:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Fail if passwords.txt exists
        run: |
          if [ -f passwords.txt ]; then
            echo "Forbidden file detected: passwords.txt"
            exit 1
          fi
          echo "No forbidden file found"
```

### Test procedure

```bash
git add .github/workflows/security_gate.yml
git commit -m "Add security gate workflow"
git push

touch passwords.txt
git add passwords.txt
git commit -m "Add forbidden file for gate test"
git push
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| First push without `passwords.txt` | Check baseline | The workflow runs successfully. | ✅ |
| Add `passwords.txt` file | Simulate policy violation | The next push triggers the failure case. | ✅ |
| `exit 1` in the check step | Actively block the build | The job is marked red and further steps do not continue. | ✅ |

---

## Notes

- **Gatekeeper:** Even simple file checks can stop accidental security breaches early on.
- **`actions/checkout@v4`:** Without checkout, the runner would have no repository content to check.
- **Scaling:** The pattern can easily be extended to cover multiple prohibited filenames or paths.

