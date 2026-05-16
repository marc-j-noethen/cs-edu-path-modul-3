# 🖥️ Locked Out - Configure a resource lock and demonstrate the block

**Course:** Cloud & Enterprise Security 2 - Governance & Monitoring | **Date:** 23 April 2026

---

## Task

**Objective:** Use the AZ-900 lab to configure a resource lock and demonstrate that a change is blocked as a result.

**Prerequisites:**
- Completion of the lab `describe-features-tools-azure-for-governance-compliance/5-exercise-configure-resource-lock`.
- Configuration of a lock on the affected resource or resource group.
- Submit a screenshot of the error message when attempting to create a storage container.

---

## Solution

### Portal steps

```text
1. Complete the AZ-900 lab on configuring resource locks.
2. Open the relevant storage account or resource group.
3. In the "Locks" or "Resource Locks" section, configure a **Read-only** lock.
4. Perform a change, for example, creating a new storage container.
5. Check that Azure blocks the action with an error message.
6. Take a screenshot of this error message.
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Set lock | Prevent unintended changes or deletions | The resource is protected at the administrative level | ✅ |
| Attempt to create container | Verify the lock’s effect in practice | The action is rejected | ✅ |
| Document error message | Secure evidence for submission | The block is visible in the screenshot | ✅ |

---

## Notes

- **Resource Lock:** Locks operate in addition to RBAC and can block changes even for authorised users.
- **Read-only vs. Delete:** A read-only lock blocks modification requests such as creating a storage container via the portal, whereas a delete lock only prevents removal.
- **Warning ⚠️:** Locks are an effective safeguard against accidental changes, but can also block legitimate maintenance operations.
