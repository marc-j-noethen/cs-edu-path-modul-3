# 🖥️ The Silencer - Cleanly exclude known secret findings via Gitleaks Ignore

**Course:** CES DevOps 1 - Code Scanning & Security Tools in CI | **Date:** 29 April 2026

---

## Task

**Objective:** To specifically resolve an existing red Gitleaks pipeline by correctly adding the known training finding to the ignore file.

**Requirements:**
- Copy the commit hash or fingerprint from the Gitleaks report in the first exercise.
- Create a `.gitleaksignore` file in the repository and enter the exact value.
- Commit the change and run the Gitleaks workflow again.
- Check that the pipeline no longer reports the known finding as an error.

---

## Solution

### Ignore file

```text
# Use the exact value from the Gitleaks report
<commit-hash-or-fingerprint-from-the-finding>
```

### Git commands

```bash
git add .gitleaksignore
git commit -m "Ignore known training secret finding"
git push
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| Adopt value from the report | Exclude only the known finding | Exactly this training finding will be ignored in future. | ✅ |
| Commit `.gitleaksignore` | Version the exception | The pipeline recognises the exception rule reproducibly in the repository. | ✅ |
| Start new scan | Validate change | The previous finding is no longer reported as an error. | ✅ |

---

## Notes

- **Ignore rules:** These are useful for legacy issues or deliberately known training artefacts.
- **Exact value:** The entry must match the report exactly, otherwise the exception will not apply.
- **Best practice:** Ignoring is only acceptable for documented special cases, not for genuine production secrets.


