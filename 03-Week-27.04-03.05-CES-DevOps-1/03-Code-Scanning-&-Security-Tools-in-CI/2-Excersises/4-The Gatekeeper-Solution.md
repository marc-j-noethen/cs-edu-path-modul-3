# 🖥️ The Gatekeeper - Configure Trivy to block builds only for critical, fixable issues

**Course:** CES DevOps 1 - Code Scanning & Security Tools in CI | **Date:** 29 April 2026

---

## Task

**Objective:** Refine the existing Trivy pipeline so that only critical and fixable risks actually block the build.

**Requirements:**
- Continue using the `.github/workflows/trivy.yml` file from the previous exercise.
- Adapt the workflow so that only `CRITICAL` is considered.
- Ignore unresolvable issues and stop the build with `exit-code: 1` for genuine hits.
- After the push, check that the gatekeeper now blocks in a more targeted but consistent manner.

---

## Solution

### Updated workflow file

```yaml
name: Trivy Dependency Scan

on: [push, workflow_dispatch]

jobs:
  trivy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: aquasecurity/trivy-action@0.28.0
        with:
          scan-type: fs
          scan-ref: .
          format: table
          severity: CRITICAL
          ignore-unfixed: true
          exit-code: '1'
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `severity: CRITICAL` | Enforce highest priority only | HIGH findings no longer automatically trigger the blocker. | ✅ |
| `ignore-unfixed: true` | Stop only fixable code | Issues without a patch do not trigger an unnecessary pipeline stop. | ✅ |
| `exit-code: 1` | Enforce policy technically | If a matching finding is detected, the build consistently turns red. | ✅ |

---

## Notes

- **Gatekeeper logic:** Less noise increases the likelihood that developers will take genuine blockers seriously.
- **Fixability:** Unfixed findings are important, but should often be tracked separately rather than blindly blocking the build.
- **Security level:** This configuration strikes a sensible balance between signal and enforcement.

