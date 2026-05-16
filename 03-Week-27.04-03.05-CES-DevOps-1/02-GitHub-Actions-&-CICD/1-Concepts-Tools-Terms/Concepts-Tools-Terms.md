# GitHub Actions & CI/CD

## 📊 Summary based on the 80/20 principle

### 1. A workflow is the security control line of your software factory
The 80/20 principle is: GitHub Actions automates checks and deployment via YAML workflows. Understand triggers, jobs, steps, runners and secrets, as this is precisely where both security benefits and pipeline risks lie.

### 2. Step-by-step core process
1. Define a workflow in a `.yml` file.
2. Specify the event that triggers it, such as `push` or `pull_request`.
3. Structure jobs and steps on a runner.
4. Use secrets and minimal permissions instead of hard-coded credentials.

### 3. Interactive mode / Tool usage
For beginners, reading a workflow file is the quickest way to understand it: triggers at the top, runners in the job, individual actions in the steps.

### 4. Key concepts with code examples
- **Workflow:** The entire automation process, defined in YAML.
- **Job and Step:** A job contains several steps on the same runner.
- **Marketplace Actions:** Reusable building blocks, but these should be maintained and pinned as a supply chain risk.
- **Secrets and Permissions:** Never include passwords in YAML; always grant minimal permissions for the workflow.

```yaml
name: security-check
on: [push]
jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run tests
        run: pytest
```

### 5. Comparison: Continuous Integration vs. Continuous Deployment
- CI checks whether new code can be built and tested.
- CD takes the next step and automatically deploys verified code to target environments.
- The more automated a pipeline is, the more critical its permissions and secrets become.

### 6. Why is this important / Benefits
A clean GitHub Action can stop vulnerabilities early on. However, an insecure GitHub Action can itself become a gateway and deploy malicious code directly into production.

**Quick-start checklist**
- ☐ I can explain workflow, job, step and runner.
- ☐ I understand the difference between a trigger and an action.
- ☐ I know why secrets do not belong in YAML.
- ☐ I am aware of the risks posed by insecure third-party actions.

**Key point**
GitHub Actions speeds up everything you define – which is why the definition itself must be particularly secure.

---

## Table 1: Tools used
| Tool | Description |
|---|---|
| GitHub Actions | Automation platform for CI/CD in GitHub |
| GitHub Actions Marketplace | Catalogue of reusable Actions |
| Encrypted Secrets | Secure storage of sensitive values for workflows |
| Runner | System on which jobs are actually executed |

## Table 2: Technical Terms
| Term | Meaning |
|---|---|
| Workflow | YAML-defined automation process |
| Trigger | Event that starts the workflow |
| Runner | Execution environment for jobs |
| Least Privilege | Grant only the minimum necessary permissions |

## Table 3: Key terms
| Term | Meaning |
|---|---|
| checkout | Retrieve code from the repository |
| push | Upload changes |
| job | Job |
| step | Individual step |


