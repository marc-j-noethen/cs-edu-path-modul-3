# 🖥️ The Injection Lockdown - Reproducing and Fixing Command Injection in GitHub Actions

**Course:** CES DevOps 1 - GitHub Actions & CI/CD | **Date:** 28 April 2026

---

## Task

**Objective:** To demonstrate a typical shell injection via unchecked GitHub inputs and subsequently secure the workflow using safe environment variable passing.

**Requirements:**
- Create an issue-triggered workflow `injection_test.yml`.
- First, recreate the vulnerable version with direct embedding of `${{ github.event.issue.title }}`.
- Run the test with a crafted issue title and observe the unintended command execution in the log.
- Then use the secure variant with `env:` and secure `printf`.

---

## Solution

### Vulnerable variant

```yaml
name: injection-test

on:
  issues:
    types: [opened]

jobs:
  vulnerable:
    runs-on: ubuntu-latest
    steps:
      - name: Vulnerable echo
        run: echo "New issue title is: ${{ github.event.issue.title }}"
```

### Secure variant

```yaml
name: injection-test

on:
  issues:
    types: [opened]

jobs:
  safe:
    runs-on: ubuntu-latest
    steps:
      - name: Safe echo
        env:
          ISSUE_TITLE: ${{ github.event.issue.title }}
        run: printf 'New issue title is: %s\n' "$ISSUE_TITLE"
```

### Exploit idea for the lab test

```text
A controlled test title such as `demo"; echo INJECTED; #` demonstrates in the vulnerable variant
that additional shell content is interpreted. In the secured variant, the same title
is output only as text.
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| Direct interpolation in the `run:` block | Understand the attack surface | The issue title can be interpreted by the shell as part of the command. | ✅ |
| Exploit with crafted title | Make injection visible | In the vulnerable run, additional external output appears in the log. | ✅ |
| `env:` + `printf` | Separate data from shell syntax | The title is treated solely as a string and is no longer evaluated. | ✅ |

---

## Notes

- **Untrusted Input:** Issue titles, PR titles and similar event fields must always be treated as potentially malicious input.
- **Safe passing:** Environment variables plus proper quoting are the robust standard solution here.
- **Log comparison:** Comparing the vulnerable and fixed variants is the real learning outcome of the exercise.
