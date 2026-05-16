# Image Security & Azure Registry

## 📊 Summary based on the 80/20 principle

### 1. Secure deployment means: build, scan, push, then deploy
The core of the 80/20 principle is this: in CD pipelines, the artefact is usually a Docker image. This image must be scanned for vulnerabilities before being pushed to a registry. Azure Container Registry stores images privately; Trivy assesses their security status.

### 2. Step-by-step core process
1. Build the Docker image from your source code.
2. Scan the base image and dependencies with Trivy.
3. Tag the image correctly and push it to Azure Container Registry.
4. Deploy only images that have passed the security check.

### 3. Interactive mode / Tool usage
In day-to-day operations, CD and image security are directly linked. The security check is not an extra step, but a fixed step before pushing to the registry.

### 4. Key concepts with code examples
- **ACR:** Private registry for enterprise images in Azure.
- **Registry, Repository, Tag:** These three levels structure where an image is located and which version it is.
- **Image Layers:** Vulnerabilities can lie in the base OS, in libraries or in the application code.
- **Trivy:** Scans OS packages, dependencies and, in some cases, misconfigurations.

```bash
docker build -t webapp:1.0 .
trivy image webapp:1.0
docker tag webapp:1.0 mycompany.azurecr.io/webapp:1.0
docker push mycompany.azurecr.io/webapp:1.0
```

### 5. Comparison: Public registry vs. private Azure Container Registry
- Public registries are convenient, but often too open for proprietary images.
- ACR provides a private, managed location for enterprise images.
- Private storage does not replace scanning, but it improves control and integration options.

### 6. Why is this important / Benefits
A single unchecked image can compromise an entire environment. Image security is therefore one of the most critical transition points from CI to secure deployment.

**Quick Start Checklist**
- ☐ I can distinguish between a registry, a repository and a tag.
- ☐ I know why an image must be scanned before being pushed.
- ☐ I understand the role of Trivy.
- ☐ I understand why ACR is important in enterprises.

**Key point**
A container is only as secure as its image – and an image is only as secure as your scan before the push.

---

## Table 1: Tools used
| Tool | Description |
|---|---|
| Azure Container Registry | Private registry for container images in Azure |
| Trivy | Scanner for container vulnerabilities and misconfigurations |
| Docker | Builds, tags and pushes images |
| GitHub Actions | Automates build, scan and push steps |

## Table 2: Technical terms
| Term | Meaning |
|---|---|
| Registry | Top-level service for storing images |
| Repository | Collection of related images |
| Tag | Version identifier of an image |
| Base Image | Base layer on which further layers are built |

## Table 3: Key Vocabulary
| Term | Meaning |
|---|---|
| scan | scan |
| push | upload |
| layer | layer |
| artifact | artifact |


