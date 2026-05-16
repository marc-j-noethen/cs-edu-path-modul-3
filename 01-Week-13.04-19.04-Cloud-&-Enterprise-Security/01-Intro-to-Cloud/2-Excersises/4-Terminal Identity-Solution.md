# 🖥️ Terminal Identity - Retrieve subscription name and role via the Azure CLI

**Course:** Cloud & Enterprise Security - Intro to Cloud | **Date:** 13 April 2026

---

## Task

**Objective:** Use the Azure Cloud Shell to determine the name of the current subscription and the assigned role using CLI commands.

**Requirements:**
- Use the Azure Cloud Shell instead of the portal.
- Display the current subscription name using the Azure CLI.
- Determine the role assigned to the logged-in user at subscription level and submit a screenshot of the command output.

---

## Solution

### Azure CLI commands

```bash
az account show --query "{Subscription:name, User:user.name, SubscriptionId:id}" -o table
az role assignment list --all --assignee "$(az account show --query user.name -o tsv)" --scope "/subscriptions/$(az account show --query id -o tsv)" --query "[].roleDefinitionName" -o table
```

### Output

```text
Subscription        User                 SubscriptionId
----------------  -------------- ---------  ------------------------------------
Azure for Students student@example.com     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Result
--------
Owner
```

---

## Analysis

| Command | Purpose | Expected information | ✓ |
|--------|-------|-----------------------|---|
| `az account show --query "{subscription:name, user:user.name, subscriptionId:id}" -o table` | Reads the active account details | Subscription name, user name and subscription ID are displayed | ✅ |
| `az role assignment list ...` | Checks the roles of the logged-in user at subscription level | One or more role names such as `Owner` or `Contributor` are displayed | ✅ |
| Screenshot of the output | Ensures the traceability of the solution | Subscription name and role are documented together | ✅ |

---

## Notes

- **az account show:** This command provides information about the currently selected Azure account and subscription.
- **Role query:** Roles are managed via Azure RBAC as role assignments at the respective scope.
- **Scope:** The scope `/subscriptions/<id>` limits the role query to the current subscription.


