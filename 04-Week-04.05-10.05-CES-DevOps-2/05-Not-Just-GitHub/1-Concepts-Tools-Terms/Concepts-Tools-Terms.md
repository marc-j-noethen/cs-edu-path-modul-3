# Not Just GitHub

## 📊 Summary based on the 80/20 principle

### 1. CI/CD is an ecosystem, not just a single tool
The essence of the 80/20 principle is this: GitHub Actions is popular, but many companies use Jenkins, GitLab CI/CD or Azure DevOps. When it comes to security, the brand name matters less than the authorisation model, plugin risks, runner access and secrets handling.

### 2. Step-by-step core process
1. Identify the pipeline system in use and its integrations.
2. Check how pipelines are defined, extended and executed.
3. Assess permissions, plugins, runners and secrets with particular rigour.
4. Plan migrations so that no new permissions or secret leaks arise.

### 3. Interactive mode / tool usage
When comparing CI/CD systems, small pipeline snippets help to quickly highlight differences in syntax, governance and extensibility.

### 4. Key concepts with code examples
- **Jenkins:** Extremely flexible, but plugin-heavy and therefore maintenance- and security-intensive.
- **GitLab CI/CD:** Highly integrated, often self-hosted platform with YAML-based pipelines.
- **Azure DevOps:** Enterprise platform with tight Microsoft and Azure integration.
- **Secrets Management:** Credentials belong in vaults or secret stores, never in the pipeline file.

```groovy
pipeline {
  agent any
  stages {
    stage('Test') {
      steps { sh 'pytest' }
    }
  }
}
```

### 5. Comparison: Jenkins vs. GitLab CI/CD vs. Azure DevOps
- Jenkins is highly customisable, but heavily reliant on plugins and their maintenance.
- GitLab CI/CD is integrated and often attractive for self-hosted environments.
- Azure DevOps is firmly anchored in Microsoft-dominated enterprise landscapes.

### 6. Why is this important / Benefits
For security teams, it is important that every pipeline has powerful access rights. The tool used changes the interface, but not the underlying risk of automated production access.

**Quick-start checklist**
- ☐ I am familiar with at least three common CI/CD platforms.
- ☐ I understand the plugin risk in Jenkins.
- ☐ I know why secrets must never be hard-coded.
- ☐ I can explain why pipeline migrations are security-critical.

**Key point**
It is not the CI/CD logo that determines security, but how strictly permissions, extensions, runners and secrets are controlled.

---

## Table 1: Tools used
| Tool | Description |
|---|---|
| Jenkins | Open-source automation server with many plugins |
| GitLab CI/CD | Pipeline platform integrated into GitLab |
| Azure DevOps | Enterprise toolchain for build and deployment |
| GitHub Secrets | Example of secure secrets in CI/CD systems |

## Table 2: Technical terms
| Term | Meaning |
|---|---|
| Plugin | Extension of a build or pipeline system |
| Runner | Execution environment for pipeline jobs |
| Insider threat | Risk posed by legitimate but misused access |
| Secret Vault | Secure storage for keys and passwords |

## Table 3: Key terms
| Term | Meaning |
|---|---|
| pipeline | Workflow |
| self-hosted | Self-operated |
| credential | Login details |
| migration | Transition |


