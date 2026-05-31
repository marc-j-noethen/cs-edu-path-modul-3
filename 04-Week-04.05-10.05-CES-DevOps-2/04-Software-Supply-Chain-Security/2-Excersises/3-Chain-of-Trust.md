# 🖥️ Chain of Trust – Simulating a supply chain attack and securing it across multiple stages

**Course:** CES DevOps 2 – Software Supply Chain Security | **Date:** 07 May 2026

---

## Task

**Objective:** Examine the supply chain across four stages: local package poisoning, integrity protection, CI defences against typosquatting, and compromise of a self-hosted runner.

**Requirements:**
- Build a harmless demo app and a malicious local training package.
- Visualise the installation hook effect and then secure it using hash pinning.
- Add an automated check to GitHub that detects suspicious PyPI packages.
- Finally, understand the difference between clean source code and a compromised runner.

---

## Solution

### Local files

```python
print("Application Started")

from setuptools import setup
from setuptools.command.install import install
from pathlib import Path

class MaliciousInstall(install):
    def run(self):
        Path.home().joinpath("pwned.txt").write_text("Infiltrated by utils_lib", encoding="utf-8")
        super().run()

setup(
    name="utils-lib",
    version="0.1.0",
    py_modules=["utils_lib"],
    cmdclass={"install": MaliciousInstall},
)

# requirements.txt
./utils_lib
```

### Stage 1 and 2 commands

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cat ~/pwned.txt

python -m build utils_lib
pip hash utils_lib/dist/utils_lib-0.1.0.tar.gz
```

### Hardened dependency reference

```text
utils-lib @ file:///absolute/path/to/utils_lib/dist/utils_lib-0.1.0.tar.gz#sha256=<your-hash>
```

### CI check for typosquatting

```yaml
name: GuardDog

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  guarddog:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: astral-sh/setup-uv@v7
      - run: uvx guarddog pypi verify requirements.txt --output-format sarif > guarddog.sarif
      - uses: github/codeql-action/upload-sarif@v3
        with:
          sarif_file: guarddog.sarif
```

### Runner compromise as controlled proof

```text
For Lab Level 4, a wrapper script on the self-hosted runner—found in the `PATH` before the real `python`,
outputting a marker such as `RUNNER_TAMPERED` and then launching the real interpreter—is sufficient.
This leaves the GitHub source code unchanged, but the build result proves the compromise of the execution environment.
```

---

## Analysis

| Step/Command | Purpose | Expected Result | ✓ |
|---|---|---|---|
| `pip install -r requirements.txt` with local package path | Demonstrate install hooks as an attack vector | The training package writes the file `pwned.txt` during installation. | ✅ |
| Hash pinning on built artefact | Technically enforce integrity | Even a minimal change to the package later leads to a hash mismatch. | ✅ |
| GuardDog in pull request | Automatically detect typosquatting | A suspicious package name such as `requesst` stands out in the PR workflow. | ✅ |
| Self-hosted runner wrapper | Make SolarWinds patterns detectable | Clean code can still produce manipulated results on compromised build infrastructure. | ✅ |

---

## Notes

- **Supply Chain:** Not only source code, but also packages, registries, runners and artefacts form part of the chain of trust.
- **GuardDog:** GuardDog provides a genuine heuristic for malicious or typosquatted PyPI packages.
- **Important:** Runner detection should be carried out as a harmless marker, not as destructive manipulation.

