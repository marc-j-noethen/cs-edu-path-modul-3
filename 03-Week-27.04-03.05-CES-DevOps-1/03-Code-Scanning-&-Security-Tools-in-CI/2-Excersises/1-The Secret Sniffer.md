# 🖥️ The Secret Sniffer - Scanning repository history with Gitleaks

**Course:** CES DevOps 1 - Code Scanning & Security Tools in CI | **Date:** 29 April 2026

---

## Task

**Objective:** Set up a Gitleaks workflow in a GitHub repository so that secrets found in the code and history are automatically visible.

**Requirements:**
- Fork the NodeGoat repository into your own GitHub account.
- Create `.github/workflows/gitleaks.yml` in the fork with the Gitleaks action code.
- Commit the workflow and then run it manually via the Actions tab.
- Verify the section containing the detected secrets in the error log.

---

## Solution

### Workflow file

```yaml
name: Gitleaks Scan

on: [push, pull_request, workflow_dispatch]

jobs:
  gitleaks:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - uses: gitleaks/gitleaks-action@v2
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Execution

```text
1. Commit the workflow directly to `main` in the forked repository.
2. In the "Actions" tab, select the `Gitleaks Scan` workflow.
3. Start `Run workflow` and wait for it to complete.
4. Open the failed run and scroll to the section `🛑 Secrets detected by Gitleaks 🛑`.
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `fetch-depth: 0` | Scan history as well as the working tree | Gitleaks sees older commits and not just the current snapshot. | ✅ |
| `workflow_dispatch` | Test the scan independently of new commits | The run can be started manually directly via GitHub. | ✅ |
| Error log | Make hits visible | The log contains specific secret findings along with the context in which they were found. | ✅ |

---

## Notes

- **Gitleaks:** The tool detects typical secret patterns in files and commit histories.
- **NodeGoat fork:** The fork serves as a safe testing ground for security workflows.
- **Red Run:** A red run is desirable here because it proves that the secret detector is working.
