# 🖥️ The Ingredient Inspector - Checking dependencies for risky packages using Trivy

**Course:** CES DevOps 1 - Code Scanning & Security Tools in CI | **Date:** 29 April 2026

---

## Task

**Objective:** Set up a Trivy scan against the forked NodeGoat repository to identify high-risk libraries and their vulnerabilities.

**Requirements:**
- Use the existing NodeGoat fork or recreate it.
- Create a workflow file `trivy.yml` for file system and dependency scans.
- Run the workflow and search the report for HIGH/CRITICAL findings.
- Identify the problematic library, the CVE and the available fix version from the report.

---

## Solution

### Workflow file

```yaml
name: Trivy Dependency Scan

on: [push, workflow_dispatch]

jobs:
  trivy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Trivy on repository filesystem
        uses: aquasecurity/trivy-action@0.28.0
        with:
          scan-type: fs
          scan-ref: .
          format: table
          severity: HIGH,CRITICAL
          exit-code: '0'
```

### Execution

```text
1. Commit `trivy.yml` to `.github/workflows/` and push it.
2. Run the workflow manually or via a push.
3. Open the table of findings in the log and note down the affected library along with the CVE and `Fixed Version`.
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `scan-type: fs` | Scan manifest and dependency files in the repo | Trivy evaluates the libraries declared in the project. | ✅ |
| `severity: HIGH,CRITICAL` | Prioritise relevant findings | The log primarily highlights serious risks. | ✅ |
| `exit-code: 0` | Inventory first, do not block yet | The workflow reports findings without immediately causing the build to fail. | ✅ |

---

## Notes

- **Trivy:** Trivy is suitable for file systems as well as container images and Infrastructure-as-Code.
- **NodeGoat:** This training project deliberately contains vulnerable components, making it ideal for dependency scans.
- **Dynamic report:** The specific CVE list depends on the current Trivy database and must be read from the actual scan.

