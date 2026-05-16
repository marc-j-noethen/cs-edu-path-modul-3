# 🖥️ Red Light Green Light – Block a vulnerable image, deploy a secure image

**Course:** CES DevOps 2 – Image Security & Azure Registry | **Date:** 06 May 2026

---

## Task

**Objective:** Using the same pipeline, cause a deliberately vulnerable build to fail once, and then successfully push a secure build to the ACR.

**Requirements:**
- Create a `vulnerable` branch with a deliberately outdated base image.
- Trigger the pipeline and observe the red scan failure.
- Switch to a current, lean base image in the `main` branch.
- Push again and verify that only the secure image ends up in the registry.

---

## Solution

### Vulnerable Dockerfile

```dockerfile
FROM python:3.6
WORKDIR /app
COPY . .
CMD ["python", "app.py"]
```

### Secure Dockerfile

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY . .
CMD ["python", "app.py"]
```

### Git workflow

```bash
git switch -c vulnerable
git add Dockerfile
git commit -m "Use vulnerable Python base image"
git push -u origin vulnerable

git switch main
git add Dockerfile
git commit -m "Switch to secure Python base image"
git push
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| Branch `vulnerable` | Run a negative test against the gatekeeper | The pipeline stops at the Trivy scan and displays a red X. | ✅ |
| `main` with `python:3.11-slim` | Provide a clean base | The scan turns green and the push to ACR is approved. | ✅ |
| Registry check | Validate the supply chain | Only the secure image is visible in ACR. | ✅ |

---

## Notes

- **Red versus green:** It is precisely this direct contrast that proves the pipeline actually makes decisions rather than merely reporting.
- **Immutable tagging:** The workflow uses `${{ github.sha }}` as a tag, making builds uniquely traceable.
- **Secure base image:** Current Slim images significantly reduce both the attack surface and the set of vulnerabilities.

