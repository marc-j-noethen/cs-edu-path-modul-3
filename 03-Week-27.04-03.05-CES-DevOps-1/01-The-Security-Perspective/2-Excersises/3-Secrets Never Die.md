# 🖥️ Secrets Never Die – Completely removing accidentally committed secrets from the history

**Course:** CES DevOps 1 – The Security Perspective | **Date:** 27 April 2026

---

## Task

**Objective:** Understand that deleted secrets remain visible without history cleanup, and completely remove the last two local commits.

**Requirements:**
- Create and commit a `config.env` file containing sample secrets.
- Delete the file and commit the deletion separately.
- Use `git show HEAD~1:config.env` to verify that the secrets are still present in the history despite being deleted.
- Completely discard the last two commits locally and display the cleaned-up history.

---

## Solution

### Git commands

```powershell
cd .\security-project

@'
DB_HOST=localhost
DB_USER=admin
DB_PASSWORD=SuperSecret123!
API_KEY=sk-1234567890abcdef
'@ | Set-Content -LiteralPath .\config.env

git add config.env
git commit -m "Add config"

Remove-Item -LiteralPath .\config.env
git add -A
git commit -m "Remove secrets"

git show HEAD~1:config.env

git reset --hard HEAD~2
git log --oneline
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `git show HEAD~1:config.env` | Verify the problem | The deleted secrets are still readable from the older revision. | ✅ |
| `Remove-Item` and `git add -A` | Stage the deletion correctly | Git records the file removal as a separate commit. | ✅ |
| `git reset --hard HEAD~2` | Clean up unpushed history | Both problematic commits disappear completely from the local history. | ✅ |
| `git log --oneline` again | Validate cleanup | The two secret-related commits no longer appear. | ✅ |

---

## Notes

- **Secrets:** A file-deletion commit does not remove a secret from the history.
- **`git reset --hard`:** This solution is only suitable for local commits that have not yet been pushed.
- **Production practice:** For commits that have already been published, rotation and history-rewrite tools such as `git filter-repo` would also be required.
