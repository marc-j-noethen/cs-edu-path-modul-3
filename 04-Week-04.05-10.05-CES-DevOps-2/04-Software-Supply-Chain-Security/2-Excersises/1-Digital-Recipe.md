# 🖥️ Digital Recipe - Generating an SBOM with Trivy and checking for transitive dependencies

**Course:** CES DevOps 2 - Software Supply Chain Security | **Date:** 07 May 2026

---

## Task

**Objective:** Generate an SBOM in CycloneDX format and then specifically identify a transitive dependency such as `Jinja2` within it.

**Requirements:**
- Start from `flask==2.0.1`, but make sure the dependency set written to `requirements.txt` also contains the resolved transitive dependencies.
- Use Trivy to generate an SBOM file `sbom.json` in CycloneDX format.
- Examine the component list and filter out `Jinja2` along with its version field.
- Save the terminal output and JSON proof for submission.

---

## Solution

### Commands

```bash
mkdir sbom-lab
cd sbom-lab
python -m venv .venv
source .venv/bin/activate
pip install flask==2.0.1
pip freeze > requirements.txt

trivy fs --format cyclonedx --output sbom.json .

jq '.components[] | select(.name=="Jinja2") | {name, version}' sbom.json
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `pip freeze > requirements.txt` | Include resolved transitive dependencies | Packages such as `Jinja2` become visible to Trivy instead of only the top-level Flask dependency. | ✅ |
| `trivy fs --format cyclonedx` | Generate standardised SBOM | The dependency landscape is written as a machine-readable CycloneDX file. | ✅ |
| Open `sbom.json` or filter with `jq` | Examine specific components | The `Jinja2` entry, including the version number, becomes visible. | ✅ |
| Component list | Make transitive dependencies visible | Indirectly included libraries also appear in the documentation. | ✅ |

---

## Notes

- **SBOM:** A Software Bill of Materials documents which components are contained within an application.
- **CycloneDX:** This is a widely used exchange format for SBOMs.
- **Trivy detail:** A bare `requirements.txt` usually contains only direct dependencies. For transitive packages such as `Jinja2`, you need a fully resolved dependency list, for example via `pip freeze`.
- **Transitive:** Indirect dependencies in particular are often the real surprise in supply chain audits.
