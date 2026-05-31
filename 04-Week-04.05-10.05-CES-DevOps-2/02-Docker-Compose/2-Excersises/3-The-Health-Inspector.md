# 🖥️ The Health Inspector - Marking a service as unhealthy using a health check

**Course:** CES DevOps 2 - Docker Compose | **Date:** 05 May 2026

---

## Task

**Objective:** Monitor an unstable service in Docker Compose using a functioning health check and make the faulty state visible.

**Requirements:**
- Define a service `flaky-api` based on `nginx:alpine`.
- Configure a health check that checks port 80 and reports `unhealthy` after two failures.
- Simulate an error by removing the default page or stopping Nginx in the container.
- Verify the resulting state using `docker compose ps`.

---

## Solution

### Compose file

```yaml
services:
  flaky-api:
    image: nginx:alpine
    restart: unless-stopped
    healthcheck:
      test: ["CMD-SHELL", "wget -q --spider http://localhost:80 || exit 1"]
      interval: 5s
      timeout: 3s
      retries: 2
```

### Error simulation

```bash
docker compose up -d
docker compose exec flaky-api rm /usr/share/nginx/html/index.html
docker compose ps
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `healthcheck.test` | Technically check service availability | As long as port 80 responds correctly, the container is considered healthy. | ✅ |
| Remove `index.html` | Generate an error | The web server no longer responds successfully to the health check. | ✅ |
| `docker compose ps` | Display status | After the failed attempts, the service appears as `(unhealthy)`. | ✅ |

---

## Notes

- **Health check:** Compose marks the container as `unhealthy`, but does not automatically restart it solely because of this status.
- **`retries: 2`:** The status only changes after two consecutive failed attempts.
- **Operational benefit:** Health checks form the basis for self-healing and orchestrated rollouts.

