# 🖥️ The Local Detective - Comparing local Trivy image scans: old vs. new

**Course:** CES DevOps 2 - Image Security & Azure Registry | **Date:** 06 May 2026

---

## Task

**Objective:** Use Trivy locally to demonstrate the significant difference in security between an outdated and a modern Python image.

**Requirements:**
- Make Trivy available locally and verify the installation.
- Scan a deliberately old image `python:3.6` and a more recent one `python:3.11-slim`.
- In the report, examine the `Total` summary and at least one `CRITICAL` finding.
- Compare both scan results directly with each other.

---

## Solution

### Commands

```bash
trivy --version

docker pull python:3.6
docker pull python:3.11-slim

trivy image python:3.6
trivy image python:3.11-slim
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| Scan of `python:3.6` | Examine a deliberately outdated reference image | The report usually shows significantly more and more severe findings. | ✅ |
| Scan of `python:3.11-slim` | Compare with a more modern base | The leaner, up-to-date image typically has significantly fewer findings. | ✅ |
| `Total` and `CRITICAL` | Compare meaningful metrics | The differences between old and new are clearly visible in the report. | ✅ |

---

## Notes

- **Trivy:** Trivy scans container images locally without an additional cloud service.
- **Image selection:** The educational value comes from the direct old-versus-new comparison.
- **Practical relevance:** Simply choosing the right base image can prevent hundreds of unnecessary vulnerabilities.

