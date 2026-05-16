# 🖥️ The Integrity Heartbeat - Successfully running the first push-based GitHub Actions workflow

**Course:** CES DevOps 1 - GitHub Actions & CI/CD | **Date:** 28 April 2026

---

## Task

**Objective:** Set up a minimal but functional CI workflow for push events and verify the successful first run.

**Requirements:**
- Create the `.github/workflows` folder in the `actions-test` repository.
- Create `heartbeat.yml` within it, which runs on `ubuntu-latest` with every push.
- Check out the code using `actions/checkout@v4` and output the heartbeat message.
- Commit and push the workflow, then check for the green tick in the Actions tab.

---

## Solution

### Workflow file

```yaml
name: heartbeat

on:
  push:

jobs:
  heartbeat:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Heartbeat message
        run: echo "Pipeline Integrity Verified - Monitoring Active"
```

### Git commands

```bash
mkdir -p .github/workflows
git add .github/workflows/heartbeat.yml
git commit -m "Add heartbeat workflow"
git push
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| Push trigger | Automatic monitoring for changes | Every new push triggers the same pipeline check. | ✅ |
| `runs-on: ubuntu-latest` | Use default runners | The job runs on GitHub Runners without requiring your own infrastructure. | ✅ |
| Echo step | Generate visible evidence in the log | The heartbeat message appears clearly in the successful run. | ✅ |

---

## Notes

- **Minimal pipeline:** A single job with a shell step is entirely sufficient for an initial CI check.
- **Green check:** The green tick confirms both syntax and successful execution.
- **Directory name:** The leading dot in `.github/workflows/` is mandatory; otherwise, GitHub will not recognise the workflow.

