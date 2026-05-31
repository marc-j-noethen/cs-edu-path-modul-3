# 🖥️ Bridge Club – Connecting two isolated Docker networks via a router container

**Course:** CES DevOps 2 – Containers & Docker | **Date:** 04 May 2026

---

## Task

**Objective:** Demonstrate that traffic between two isolated bridge networks can be relayed via a deliberately interposed router container.

**Requirements:**
- Create the two networks `public-zone` and `private-zone`.
- Start a `victim-server` only in `private-zone` and have it listen on port `4444`.
- Use an `internet-client` only in `public-zone`.
- Connect a `router` container to both networks and configure it as a TCP relay.

---

## Solution

### Docker commands

```bash
docker network create public-zone
docker network create private-zone

docker run -d --name victim-server --network private-zone alpine sh -c "apk add --no-cache busybox-extras && while true; do nc -l -p 4444; done"

docker run -d --name router --network public-zone alpine sh -c "apk add --no-cache socat && socat TCP-LISTEN:4444,fork TCP:victim-server:4444"
docker network connect private-zone router

docker run --rm --name internet-client --network public-zone alpine sh -c "apk add --no-cache busybox-extras && echo hello from public-zone | nc router 4444"

docker logs victim-server
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| Two bridge networks | Separate communication zones | The client cannot directly resolve or reach the private destination. | ✅ |
| `router` with both networks | Create a controlled switching point | The router sees both sides and can forward traffic. | ✅ |
| `socat TCP-LISTEN... TCP:victim-server:4444` | Use relay instead of direct route | Messages from `public-zone` reach the internal listener. | ✅ |
| `docker logs victim-server` | Verify success | The message sent by the `internet-client` is visible on the target server. | ✅ |

---

## Notes

- **Network isolation:** Without the router, `victim-server` remains isolated from the `public-zone`.
- **Router container:** This is deliberately a relay and not a full-fledged Layer 3 router.
- **Security relevance:** The exercise demonstrates lateral movement pathways in container environments.



