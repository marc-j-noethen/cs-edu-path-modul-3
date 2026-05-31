# 🖥️ The Pandora Variable – Understanding and securing CVE-2025-62725 in a lab environment

**Course:** CES DevOps 2 – Docker Compose | **Date:** 05 May 2026

---

## Task

**Objective:** To technically classify the Docker Compose vulnerability CVE-2025-62725 in an isolated lab environment and clearly identify the effective protective measures.

**Requirements:**
- Understand that the vulnerability affects remote OCI Compose artefacts with manipulated path specifications.
- Deploy a vulnerable Compose version lower than `2.40.2` only in an isolated test environment.
- Conceptually reproduce the attack using a malicious OCI artefact that escapes from the cache when loaded.
- Subsequently, consistently update to `2.40.2+` and use only trusted artefacts, preferably those pinned by digest.

---

## Solution

### Lab Procedure

```text
1. Verify that the lab environment is using a vulnerable Compose version `< 2.40.2`.
2. Load the test artefact exclusively from a controlled test registry.
3. Even an apparently read-only command such as
   `docker compose -f oci://registry.example.com/lab-stack:latest config`
   can trigger the malicious file write operation if the artefact has been tampered with.
4. Then update Compose to `2.40.2` or newer and repeat the test using the same reference.
5. Once patched, the path traversal should no longer work.
```

### Mitigation measures

```text
- Upgrade Docker Compose to at least `2.40.2`.
- Preferably reference OCI artefacts via digest rather than via a mutable tag.
- Use only private or clearly controlled registries for remote Compose artefacts.
- Carefully review all Compose warning and confirmation dialogues regarding variables, includes and remote sources.
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| Compose `< 2.40.2` | Narrow down affected version | Only the vulnerable builds are relevant for the lab test. | ✅ |
| Remote OCI reference via `oci://` | Correctly address the attack surface | The test precisely matches the described remote artefact mechanism. | ✅ |
| Upgrade to `2.40.2+` | Apply vendor fix | The path traversal vulnerability is fixed in the patched version. | ✅ |

---

## Notes

- **CVE-2025-62725:** The official description refers to path trust in Remote OCI Compose artefacts and a fix in `2.40.2`.
- **Digest pinning:** Tags can be changed, but digests cannot. This significantly reduces the risk of undetected artefact swapping.
- **Security boundary:** This exercise belongs exclusively in an isolated test environment and not on production runners or developer hosts.

