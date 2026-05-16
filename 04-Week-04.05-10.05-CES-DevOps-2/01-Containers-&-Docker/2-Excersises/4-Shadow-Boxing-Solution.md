# 🖥️ Shadow Boxing – Reproducing and securing CVE-2019-5021 in Alpine

**Course:** CES DevOps 2 – Containers & Docker | **Date:** 04 May 2026

---

## Task

**Objective:** Reproduce the Alpine Docker vulnerability CVE-2019-5021 using an old image plus the `shadow` package, and then mitigate it in accordance with the advisory.

**Requirements:**
- Use a vulnerable Alpine image such as `alpine:3.5` with `shadow` and `linux-pam`.
- Create a non-root user and simulate the password-less switch to `root` via `su -`.
- Then apply the Alpine recommendation and explicitly block root login.
- Rebuild and demonstrate that `su -` no longer works afterwards.

---

## Solution

### Vulnerable Dockerfile

```dockerfile
FROM alpine:3.5
RUN apk add --no-cache shadow linux-pam \
 && adduser -D analyst
USER analyst
CMD ["/bin/sh"]
```

### Secured Dockerfile

```dockerfile
FROM alpine:3.5
RUN apk add --no-cache shadow linux-pam \
 && sed -i -e 's/^root::/root:!:/' /etc/shadow \
 && adduser -D analyst
USER analyst
CMD ["/bin/sh"]
```

### Test commands

```bash
docker build -t alpine-shadow-vuln -f Dockerfile.vuln .
docker run -it --rm alpine-shadow-vuln
# inside the container: su -

docker build -t alpine-shadow-fixed -f Dockerfile.fixed .
docker run -it --rm alpine-shadow-fixed
# inside the container: su -
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| Old `alpine:3.5` + `shadow` | Reproduce vulnerability | The root account ends up in a problematic state with an empty password. | ✅ |
| `su -` as non-root user | Demonstrate privilege escalation | Root access without a password is possible in the vulnerable container. | ✅ |
| `sed -i -e 's/^root::/root:!:/' /etc/shadow` | Apply advisory fix | The root login is explicitly blocked. | ✅ |
| New test in the fixed image | Verify mitigation | The password-less switch to `root` now fails. | ✅ |

---

## Notes

- **CVE-2019-5021:** According to the Alpine advisory, the issue affects older official Docker images using `shadow` or `linux-pam`.
- **Realistic workaround:** In practice, updating to a supported Alpine version is the cleanest approach.
- **Learning objective:** This exercise demonstrates how significantly the base image and additional packages can alter a container’s security status.

