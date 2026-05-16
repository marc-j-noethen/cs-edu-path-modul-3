# 🖥️ Putting On The Gloves - Complete the Docker Compose section of the workshop

**Course:** CES DevOps 2 - Docker Compose | **Date:** 05 May 2026

---

## Task

**Objective:** Complete the Docker workshop from the Compose section onwards and document the running application plus Compose logs in a clear and comprehensible manner.

**Requirements:**
- Complete the official workshop from Part 7 `Use Docker Compose` onwards.
- Start the Todo app via Compose and check it in the browser.
- Monitor the stack logs in parallel using `docker compose logs -f`.
- Create both an app screenshot and a log screenshot for submission.

---

## Solution

### Key Compose commands

```bash
docker compose up -d
docker compose ps
docker compose logs -f
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `docker compose up -d` | Start multiple services together | The application and its dependencies run as a stack. | ✅ |
| `docker compose ps` | Check container status | All relevant services are listed as `running` in the Compose project. | ✅ |
| `docker compose logs -f` | Monitor runtime behaviour live | The logs centrally display start-up, requests and any errors. | ✅ |

---

## Notes

- **Compose:** Docker Compose describes and starts related multi-container applications declaratively.
- **Workshop focus:** From Part 7 onwards, the focus shifts from individual containers to a structured stack.
- **Submission:** The combination of a running app and logs demonstrates both functionality and observability.

