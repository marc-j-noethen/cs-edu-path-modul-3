# 🖥️ Child’s Play – Editing users in an Entra administrative unit and checking audit logs

**Course:** Cloud & Enterprise Security – IAM Basics | **Date:** 16 April 2026

---

## Task

**Objective:** Edit a user record, reset a password, and subsequently verify the actions in the audit logs within the `Playground` administrative unit in Microsoft Entra ID.

**Prerequisites:**
- Log in to `https://entra.microsoft.com/` and navigate to the `Playground` administrative unit.
- Update an existing user profile with job title, department and location, and perform a password reset.
- Submit a screenshot of the audit logs showing your account name and the actions performed.

---

## Solution

### Entra Portal steps

```text
1. Log in at https://entra.microsoft.com/.
2. Go to "Identity" > "Roles & Administrators" > "Administrative Units".
3. Open the "Playground" administrative unit.
4. In the "Users" section, select an existing user.
5. Customise the profile via "Edit":
   - Job title: e.g. Cloud Architect
   - Department: e.g. Project Blue
   - Location: e.g. Berlin
6. Save changes.
7. For the same user, select "Reset password" and complete the process.
8. Go to "Activity" > "Audit logs".
9. Use filters to search for the actions "Update user" and "Reset user password".
10. Take a screenshot showing the account name and both actions.
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Open the `Playground` administrative unit | Use the scope of the assigned admin rights | Access to users within the AU is possible | ✅ |
| Edit user profile | Make an administrative change to the record | Job title, department and location are updated | ✅ |
| Reset password | Test another user management function | The reset is processed successfully | ✅ |
| Check audit logs | Confirm traceability of actions | Both actions appear in the log under your own account | ✅ |

---

## Notes

- **Administrative Unit:** Administrative Units restrict Entra administration rights to defined subsets of users or groups.
- **Audit logs:** Audit logs document security-related administrative actions and are important for traceability and control.
- **Warning ⚠️:** As users are shared, only the required practice changes should be made.

