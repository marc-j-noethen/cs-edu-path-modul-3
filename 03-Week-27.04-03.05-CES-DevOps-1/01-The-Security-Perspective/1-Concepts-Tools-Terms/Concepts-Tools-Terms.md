# The Security Perspective

## 📊 Summary based on the 80/20 principle

### 1. DevOps accelerates delivery; DevSecOps embeds security into the process
The essence of the 80/20 principle is this: DevOps breaks down the silos between development and operations through automation. DevSecOps shifts security controls to the left so that vulnerabilities are detected earlier and more cost-effectively.

### 2. Step-by-step core process
1. Plan small changes rather than large, risky releases.
2. Automate build, test, release and deployment in a single pipeline.
3. Integrate security checks right from the planning, coding and build stages.
4. Use Git and pull requests as an audit trail and for quality control.

### 3. Interactive mode / tool usage
For security, Git is more than just version control: it is a log, a checkpoint and often the starting point of every pipeline. This is precisely where reviews and security checks come into play.

### 4. Key concepts with code examples
- **Infinity Loop:** Plan, Code, Build, Test, Release, Deploy, Operate and Monitor form a closed feedback loop.
- **Shift Left:** Security checks take place as early as possible, not just shortly before production.
- **Git as the source of truth:** Everything important is versioned in the repository.
- **Secrets in Git:** Hard-coded credentials are a classic and highly critical attack vector.

```bash
git checkout -b feature/harden-login
git add .
git commit -m "Add MFA validation"
git push origin feature/harden-login
```

### 5. Comparison: DevOps vs. DevSecOps
- DevOps primarily optimises speed, reliability and collaboration.
- DevSecOps adopts these goals but adds systematic security checks at every stage.
- The difference is not an additional tool, but a security process integrated at an early stage.

### 6. Why is this important / Benefits
Pipelines are a primary target for attacks today. Anyone who understands DevOps recognises where security controls need to be placed and how supply chain attacks become possible in the first place.

**Quick-start checklist**
- ☐ I can explain the DevOps Infinity Loop.
- ☐ I understand the ‘shift left’ concept.
- ☐ I know why Git is relevant to security.
- ☐ I am aware of the risk of secrets in the repository.

**Key point**
Secure software is not created by a final checkpoint, but through early, repeatable security work throughout the entire pipeline.

---

## Table 1: Tools used
| Tool | Meaning |
|---|---|
| Git | Version controls changes and creates an audit trail |
| Pull Request | Place for review, discussion and automated checks |
| CI/CD pipeline | Automates build, test and delivery |
| MFA | Strengthens identity protection for security-critical accounts |

## Table 2: Technical terms
| Term | Meaning |
|---|---|
| DevOps | Culture and practice of closer collaboration between Dev and Ops |
| DevSecOps | Integration of security into every phase of the pipeline |
| Shift Left | Placing security controls early in the process |
| Audit Trail | Traceable history of changes |

## Table 3: Key Vocabulary
| Term | Meaning |
|---|---|
| release | publish |
| rollback | Undo a change |
| pipeline | Sequence of automated steps |
| review | Check |


