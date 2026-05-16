# 🖥️ GitCommands - Understanding the command reference for branching and merging

**Course:** CES DevOps 1 - The Security Perspective | **Date:** 28 April 2026

---

## Task

**Objective:** Document the command sequence from the branching exercise as a clear reference for branch creation, merging and history checking.

**Requirements:**
- Change to the existing `security-project`.
- If necessary, set the main branch to `main`.
- Create, modify, commit, switch back and merge a branch.
- Finally, check the complete history, including branch decoration.

---

## Solution

### Command sequence

```bash
cd security-project
git branch
git branch -M main
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
| `git branch -M main` | Standardise branch names | The main branch has the expected name `main`. | ✅ |
| `git switch -c feature-auth` | Create a feature branch | New changes are isolated in the feature branch. | ✅ |
| `git merge --no-ff feature-auth` | Document feature integration | The merge is visible in the history and has not been fast-forwarded. | ✅ |
| `git log --graph --decorate --all` | Final check | The branch topology is clear as a graph. | ✅ |

---

## Notes

- **Command reference:** This file is not a summary of theory, but the appropriate sequence of commands for the exercise.
- **`git branch`:** Helps to check the active branch and existing branches before and after the merge.
- **History graph:** Especially in learning exercises, the graphical view is often the quickest way to check plausibility.
