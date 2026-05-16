# Software Supply Chain Security

## 📊 Summary based on the 80/20 principle

### 1. It is not just your code that poses a risk, but the entire supply chain
The essence of the 80/20 principle is this: every application consists of source code, dependencies, a build pipeline and distribution. If any of these components is compromised, the end product is no longer trustworthy.

### 2. Step-by-step core process
1. Identify the components of your supply chain: code, dependencies, build and distribution.
2. Take stock of the packages used and transitive dependencies.
3. Generate an SBOM so that affected components can be quickly identified.
4. Be aware of attack vectors such as typosquatting, account takeover and build system compromise.

### 3. Interactive mode / tool usage
A very practical starting point is to generate an SBOM. This reveals just how much third-party code is actually contained within what appears to be a small application.

### 4. Key concepts with code examples
- **Dependencies:** An imported package often brings with it many further transitive packages.
- **SBOM:** A machine-readable list of ingredients for your software components.
- **Typosquatting:** Malicious packages with similar-sounding names lie in wait for typos.
- **Build Compromise:** Even if the source code appears clean, the pipeline can compromise the end product.

```bash
pip install cyclonedx-bom
cyclonedx-py requirements requirements.txt --output-format json -o sbom.json
```

### 5. Comparison: Manual search vs. SBOM
- Without an SBOM, a team must manually search through libraries across many projects in the event of an incident.
- With an SBOM, an affected component can be identified immediately across the board.
- An SBOM does not replace patching, but it massively speeds up the response.

### 6. Why is this important / Benefits
Supply chain security is essential today because modern software rarely consists solely of in-house code. The point of attack often lies in the supplier network, not in the app interface.

**Quick-start checklist**
- ☐ I can list the four core components of the software supply chain.
- ☐ I know what transitive dependencies are.
- ☐ I can explain the purpose of an SBOM.
- ☐ I know at least two typical supply chain attack vectors.

**Key point**
If you only check your own code, you are only protecting a small part of your actual application.

---

## Table 1: Tools used
| Tool | Description |
|---|---|
| GitHub | Stores source code and triggers build processes |
| cyclonedx-bom | Generates an SBOM from Python dependencies |
| Package Registry | Source of external libraries such as PyPI or npm |
| CI/CD pipeline | Compiles, tests and deploys software artefacts |

## Table 2: Technical terms
| Term | Meaning |
|---|---|
| SBOM | Software Bill of Materials as a list of software components |
| Transitive dependency | Indirect dependency of a directly used package |
| Typosquatting | Misuse of similar package names |
| Supply Chain | Supply chain of all components up to delivery |

## Table 3: Key Vocabulary
| Term | Meaning |
|---|---|
| dependency | Dependency |
| maintainer | Maintainer |
| artifact | Build result |
| compromise | Compromise |


