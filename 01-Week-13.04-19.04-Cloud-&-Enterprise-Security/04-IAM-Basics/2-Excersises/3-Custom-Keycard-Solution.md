# 🖥️ Custom Keycard - Create and validate a custom RBAC role using the Azure CLI

**Course:** Cloud & Enterprise Security - IAM Basics | **Date:** 16 April 2026

---

## Task

**Objective:** Define, create, verify via CLI and the portal, and then delete a custom Azure role named `Junior VM Operator`.

**Requirements:**
- Create a JSON role definition with permissions to read all resources and to start, stop and restart VMs.
- Insert your own subscription ID into `AssignableScopes` and create the role using `az role definition create`.
- Validate the role via the CLI and in the portal, and then delete it using `az role definition delete`.

---

## Solution

### Role definition

```json
{
  "Name": "Junior VM Operator",
  "IsCustom": true,
  "Description": "Can read, start, stop and restart all VMs, but cannot create or delete them.",
  "Actions": [
    "*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Compute/virtualMachines/powerOff/action",
    "Microsoft.Compute/virtualMachines/deallocate/action"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/YOUR_SUBSCRIPTION_ID_GOES_HERE"
  ]
}
```

### Azure CLI commands

```bash
az account show --query "{Name:name, SubscriptionId:id}" -o table

az role definition create --role-definition ./JuniorVmOperator.json

az role definition list --name "Junior VM Operator" -o json

az role definition delete --name "Junior VM Operator"
```

### Output

```text
Name                 SubscriptionId
-------------------  ------------------------------------
Azure for Students   xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

[
  {
    "roleName": "Junior VM Operator",
    "roleType": "CustomRole",
    "permissions": [
      {
        "actions": [
          "*/read",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action",
          "Microsoft.Compute/virtualMachines/powerOff/action",
          "Microsoft.Compute/virtualMachines/deallocate/action"
        ]
      }
    ]
  }
]
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|----------------|-------|---------------------|---|
| `az account show --query "{Name:name, SubscriptionId:id}" -o table` | Reads the current subscription ID | The ID can be used in `AssignableScopes` | ✅ |
| Populate JSON with `Actions` | Define permissions according to the least privilege principle | The role contains only read and start/stop/restart actions | ✅ |
| `az role definition create --role-definition ./JuniorVmOperator.json` | Creates the custom role | The `Junior VM Operator` role is imported | ✅ |
| `az role definition list --name "Junior VM Operator" -o json` | Checks the new role definition via CLI | The actions match the JSON file | ✅ |
| Portal check under `IAM` > `Roles` | Visual check in the Azure portal | The role appears as `CustomRole` with the expected permissions | ✅ |
| `az role definition delete --name "Junior VM Operator"` | Cleans up the exercise resources | The custom role is removed | ✅ |

---

## Notes

- **Least Privilege:** A custom role should only contain the exact actions that are truly necessary for the task.
- **Custom Role:** Custom roles complement Azure built-in roles when standard roles contain too many or too few permissions.
- **Warning ⚠️:** `AssignableScopes` must contain a valid subscription ID, otherwise the role cannot be created successfully.

