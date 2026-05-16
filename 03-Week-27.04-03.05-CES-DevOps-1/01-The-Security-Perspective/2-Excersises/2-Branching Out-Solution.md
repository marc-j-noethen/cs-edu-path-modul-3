# 🖥️ Branching Out – Creating a feature branch and merging it safely back into `main`

**Course:** CES DevOps 1 – The Security Perspective | **Date:**  27 April 2026

---

## Task

**Objective:** Implement a clean branching strategy using a feature branch, a separate commit and a controlled merge into `main`.

**Requirements:**
- Continue using the existing `security-project` repository from Exercise 1.
- Create a branch called `feature-auth` and create `auth.py` there with placeholder code.
- Commit the change, switch back to `main` and merge the branch.
- Make the merge traceable using a graphical log view.

---

## Solution

### Git commands

```bash
cd security-project

git switch -c feature-auth
cat > auth.py <<'EOF'
def authenticate(user, password):
    return False
EOF

git add auth.py
git commit -m "Add auth stub"

git switch main
git merge --no-ff feature-auth -m "Merge feature-auth"

git log --oneline --graph --decorate --all
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `git switch -c feature-auth` | Isolate feature work | The change is created separately from the main branch. | ✅ |
| Commit to `feature-auth` | Document a standalone work step | The branch contains only the Auth stub as the change. | ✅ |
| `git merge --no-ff feature-auth` | Make the merge explicitly visible | A merge commit documents the integration. | ✅ |
| `git log --graph --decorate --all` | Check branch structure | The history clearly shows the branch, merge and branch pointers. | ✅ |

---

## Notes

- **Branch:** Branches separate parallel work without immediately altering the main branch.
- **`--no-ff`:** This ensures the merge remains visible as a separate event.
- **Auth stub:** Placeholder code is sufficient here, as the focus is on the Git workflow rather than implementation logic.


