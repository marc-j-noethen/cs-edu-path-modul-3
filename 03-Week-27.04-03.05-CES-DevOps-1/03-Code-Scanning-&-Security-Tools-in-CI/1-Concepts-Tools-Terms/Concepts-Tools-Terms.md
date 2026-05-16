# Code Scanning & Security Tools in CI

## 📊 Summary based on the 80/20 principle

### 1. Four types of scan cover different aspects of the risk
The essence of the 80/20 principle is this: SAST checks the code, DAST checks the running application, secret scanning identifies credentials, and SCA assesses dependencies. Only together do they provide a realistic picture of vulnerabilities in the pipeline.

### 2. Step-by-step core process
1. Scan source code early with SAST, even before code is merged.
2. Scan running applications on a trial basis with DAST.
3. Automatically search for secrets in commits and pull requests.
4. Use SCA to check which libraries and versions your application uses.

### 3. Interactive mode / Tool usage
In a CI pipeline, scanners should run as fixed gatekeepers. This way, the pipeline stops immediately if a critical vulnerability or a secret is found.

### 4. Key concepts with code examples
- **SAST:** Finds code errors and insecure patterns without running the application.
- **DAST:** Tests the application from the outside and detects real attack surfaces.
- **Secret Scanning:** Finds tokens, passwords or API keys that have been accidentally checked in.
- **SCA:** Assesses open-source dependencies and their known CVEs.

```yaml
name: codeql-scan
on: [pull_request]
jobs:
  analyze:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: github/codeql-action/init@v3
        with:
          languages: python
      - uses: github/codeql-action/analyze@v3
```

### 5. Comparison: SAST vs. DAST
- SAST looks at the code and finds issues before runtime.
- DAST tests the running application and detects how it behaves in real life.
- SAST is earlier and cheaper; DAST is more realistic but comes later in the process.

### 6. Why is this important / Benefits
If you only use one type of scan, you only see part of the problem. For DevSecOps, it’s all about combination and automation.

**Quick-start checklist**
- ☐ I can distinguish between SAST, DAST, secret scanning and SCA.
- ☐ I know at which point in the pipeline SAST is useful.
- ☐ I understand why secret scanning is mandatory.
- ☐ I can explain why SCA is important in open-source-heavy environments.

**Key point**
Security scans work best as a team: code, runtime, secrets and dependencies must be assessed together.

---

## Table 1: Tools used
| Tool | Description |
|---|---|
| CodeQL | SAST tool for source code analysis |
| DAST scanner | Checks running web applications from the outside |
| Secret scanning | Searches for tokens and passwords in the code |
| SCA tools | Assess libraries and known vulnerabilities |

## Table 2: Technical terms
| Term | Meaning |
|---|---|
| SAST | Static Application Security Testing |
| DAST | Dynamic Application Security Testing |
| Secret | Sensitive access value such as a token or password |
| SCA | Software Composition Analysis for dependencies |

## Table 3: Key terms
| Term | Meaning |
|---|---|
| vulnerability | Vulnerability |
| dependency | Dependency |
| token | Access token |
| analyse | Analyse |


