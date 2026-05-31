# 🖥️ abRBACadabra - Granting and verifying Reader access via IAM

**Course:** Cloud & Enterprise Security - IAM Basics | **Date:** 16 April 2026

---

## Task

**Objective:** Use `Access control (IAM)` for an Azure resource to check that a fellow student does not yet have access, then assign them the `Reader` role and verify the successful authorisation again.

**Requirements:**
- Complete the quickstart guide `Check access for a user`.
- Assign the `Reader` role to a selected classmate on the chosen resource.
- Submit a screenshot of the `Check access` section showing successful authorisation.

---

## Solution

### Portal steps

```text
1. If necessary, first create an Azure resource, for example a resource group.
2. Open the selected resource in the Azure portal.
3. Go to "Access control (IAM)" > "Check access".
4. Search for a classmate and verify that they currently have no access.
5. Go to the "Role assignments" tab or to "Add" > "Add role assignment".
6. Select the "Reader" role.
7. Search for the same fellow student and complete the assignment by clicking "Review + assign".
8. Return to "Check access".
9. Search for the same person again and check the new role.
10. Take a screenshot of the "Check access" section showing the successful assignment.
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Initial check in `Check access` | Determine initial state | The fellow student does not yet have access to this resource | ✅ |
| Assign `Reader` role | Grant minimal read access | The role assignment is created successfully | ✅ |
| Second check in `Check access` | Verify the change has taken effect | The `Reader` role appears for the selected person | ✅ |
| Take a screenshot | Document the result | The successful authorisation is clearly visible | ✅ |

## Questions & Answers

**Q:** How can you prove that the role assignment was successful?

**A:** 
- Before the assignment, `Check Access` shows that no access is available.
- After the assignment, the same section shows the `Reader` role for the same person.
- It is precisely this before-and-after evidence that forms the basis for the screenshot to be submitted.

---

## Notes

- **Azure RBAC:** Azure Role-Based Access Control controls who is permitted to perform which actions at which level.
- **Reader:** The `Reader` role allows resources to be viewed, but not modified.
- **Scope:** The effect of the role assignment depends on the resource, resource group or subscription to which it is applied.

