# 🖥️ Git Basics Bootcamp – Initialising a Git repository and building a commit history

**Course:** CES DevOps 1 – The Security Perspective | **Date:**  27 April 2026

---

## Task

**Objective:** To work through the basic Git workflow – including initialisation, staging, committing and version control – in a clear and practical manner.

**Requirements:**
- Create a new repository named `security-project` and initialise it as a Git repository.
- Create a `README.md` and commit it with a meaningful commit message.
- Then check in `config.yaml` and `scanner.py` together in a second commit.
- Finally, verify the history with `git log --oneline`.

---

## Solution

### Git commands

```bash
mkdir security-project
cd security-project

git init
git branch -M main

cat > README.md <<'EOF'
# Sentinel Scanner
A lightweight demo security scanner for training purposes.
EOF

git add README.md
git commit -m "Add project README"

touch config.yaml scanner.py
git add config.yaml scanner.py
git commit -m "Add scanner skeleton and config"

git log --oneline
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `git init` and `git branch -M main` | Initialise repository cleanly | The new project is versioned and the main branch is called `main`. | ✅ |
| `git add README.md` + first commit | Document project basics | The README appears as the first commit in the history. | ✅ |
| `touch config.yaml scanner.py` + second commit | Check in additional files collectively | Both files are included together in the second commit. | ✅ |
| `git log --oneline` | Generate commit history | The history shows exactly two commits in short form. | ✅ |

---

## Notes

- **Git:** Git stores changes as a traceable history of commits.
- **Staging Area:** With `git add`, you specify exactly what goes into the next commit.
- **`git log --oneline`:** The short view is ideal as quick proof of the commit.

