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

```powershell
mkdir security-project
cd security-project
git init
git branch -M main

Set-Content -LiteralPath .\README.md -Value '# Security Project'
Add-Content -LiteralPath .\README.md -Value 'A fictional tool for scanning suspicious files.'

git add README.md
git commit -m "Add README for fictional security tool"

Set-Content -LiteralPath .\config.yaml -Value 'scan_mode: demo'
Set-Content -LiteralPath .\scanner.py -Value 'print("scanner placeholder")'

git add config.yaml scanner.py
git commit -m "Add config and scanner placeholders"

git log --oneline
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `git init` and `git branch -M main` | Initialise repository cleanly | The new project is versioned and the main branch is called `main`. | ✅ |
| `Set-Content` for `README.md` | Document project basics | The README contains the project title and short description. | ✅ |
| `git add README.md` + first commit | Record the first milestone | The README appears as the first commit in the history. | ✅ |
| `Set-Content` for `config.yaml` and `scanner.py` | Create both placeholders | Both files are ready for the second commit. | ✅ |
| `git add config.yaml scanner.py` + second commit | Check in additional files collectively | Both files are included together in the second commit. | ✅ |
| `git log --oneline` | Generate commit history | The history shows exactly two commits in short form. | ✅ |

---

## Notes

- **Git:** Git stores changes as a traceable history of commits.
- **Staging Area:** With `git add`, you specify exactly what goes into the next commit.
- **`git log --oneline`:** The short view is ideal as quick proof of the commit.
