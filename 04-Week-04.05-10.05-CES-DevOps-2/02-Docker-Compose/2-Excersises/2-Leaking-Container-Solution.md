# 🖥️ Leaking Container - Keeping secrets out of command-line arguments

**Course:** CES DevOps 2 - Docker Compose | **Date:** 05 May 2026

---

## Task

**Objective:** Identify the insecure passing of a database password via Compose interpolation and refactor the code so that the plaintext no longer appears in `Config.Cmd`.

**Requirements:**
- Create a `.env` file with `DATABASE_PASSWORD=SuperSecret123`.
- Understand the insecure variant using `${DATABASE_PASSWORD}` in the `command`.
- Switch to a variant using `env_file` and shell expansion within the container.
- Use `docker inspect` to verify that the password is no longer visible in the command arguments.

---

## Solution

### Secure Compose file

```yaml
services:
  app-safe:
    image: alpine
    env_file:
      - .env
    command: ["sh", "-c", "echo \"$$DATABASE_PASSWORD\" > /tmp/runtime_secret.txt && sleep infinity"]
```

### Test commands

```bash
docker compose up -d
docker inspect <container-id>
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `env_file:` | Decouple secret from command | The password is provided as a runtime environment variable. | ✅ |
| `$$DATABASE_PASSWORD` | Avoid Compose interpolation | Compose does not write the plain text into `command`, but allows the shell in the container to expand it. | ✅ |
| `docker inspect` on `Config. Cmd` | Perform leak check | The command arguments now only contain the variable, no longer `SuperSecret123`. | ✅ |

---

## Notes

- **Insecure base version:** `${DATABASE_PASSWORD}` would already be expanded during Compose parsing and appear in the container command.
- **Safer is better:** For real production secrets, Docker Secrets or a secret manager would be preferable.
- **Metadata leakage:** This exercise focuses specifically on leakage via command arguments and not on any other metadata sources.

