# 🖥️ Rotten Apples – Identifying vulnerable npm dependencies and their fixed versions

**Course:** CES DevOps 2 – Software Supply Chain Security | **Date:** 07 May 2026

---

## Task

**Objective:** Use Trivy to scan a deliberately outdated `lodash` version and derive the most critical recommendations for action from the report.

**Requirements:**
- Create a directory containing `lodash: 4.17.15` and generate an npm lock file before scanning.
- Run a Trivy filesystem scan against the project.
- Identify the highest severity, the associated CVE and the recommended fix version in the report.
- Use the table with the relevant columns as proof of submission.

---

## Solution

### File and scan

```bash
mkdir vulnerable-app
cd vulnerable-app
npm init -y
npm install lodash@4.17.15 --package-lock-only

trivy fs --severity HIGH,CRITICAL .
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `lodash 4.17.15` | Deliberately using an old library | The scan finds several known security issues. | ✅ |
| `npm install ... --package-lock-only` | Generate the lock file Trivy expects for npm dependency analysis | The exact vulnerable version is pinned in `package-lock.json`. | ✅ |
| `trivy fs --severity HIGH,CRITICAL .` | Focus on relevant findings | The output displays CVE IDs and `Fixed Version` directly in the terminal. | ✅ |
| Evaluate report | Derive actionable fix | You can derive the next secure package version directly from the finding. | ✅ |

---

## Notes

- **Trivy report:** The specific CVE list may change with the updated vulnerability database.
- **npm detail:** Trivy uses npm lock files such as `package-lock.json` for reliable vulnerability detection in repository scans.
- **Fixed Version:** It is the `Fixed Version` column in particular that turns a finding into a concrete to-do recommendation.
- **npm supply chain:** Outdated libraries are one of the most common sources of vulnerabilities in build pipelines.
