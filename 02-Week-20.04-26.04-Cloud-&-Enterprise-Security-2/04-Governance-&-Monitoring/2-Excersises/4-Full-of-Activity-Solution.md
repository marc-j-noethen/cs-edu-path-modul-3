# 🖥️ Full of Activity - Re-enacting Azure activity logs and analysing them with KQL

**Course:** Cloud & Enterprise Security 2 - Governance & Monitoring | **Date:** 23 April 2026

---

## Task

**Objective:** Send Azure activity logs to a Log Analytics workspace, generate appropriate actions and formulate your own KQL queries for four scenarios.

**Requirements:**
- Create a Log Analytics workspace and export the activity logs to it.
- Perform the four required actions in Azure so that at least one log entry is generated.
- Create KQL queries for VM shutdown, deployment creation, VM creation and successfully listing storage account keys.

---

## Solution

### Preparation

```text
1. Create a Log Analytics workspace.
2. Under Azure Monitor > Activity log > Export Activity Logs, create a diagnostic setting.
3. Send the activity logs to the workspace.
4. Perform each of the required actions in Azure at least once.
```

### KQL queries

```kusto
AzureActivity
| where TimeGenerated > ago (10m)
| where OperationNameValue =~ "Microsoft.Compute/virtualMachines/deallocate/action"
   or OperationNameValue =~ "Microsoft.Compute/virtualMachines/powerOff/action"
| where ActivityStatusValue =~ "Succeeded"
| project TimeGenerated, ResourceGroup, Resource, OperationNameValue, Caller, ActivityStatusValue
```

```kusto
AzureActivity
| where TimeGenerated > ago(24h)
| where OperationNameValue =~ "Microsoft.Resources/deployments/write"
| where ActivityStatusValue =~ "Succeeded"
| project TimeGenerated, ResourceGroup, ResourceId, Caller, ActivityStatusValue
```

```kusto
AzureActivity
| where TimeGenerated > ago (24h)
| where OperationNameValue =~ "Microsoft.Compute/virtualMachines/write"
| where ActivityStatusValue =~ "Succeeded"
| project TimeGenerated, ResourceGroup, Resource, Caller, ActivityStatusValue
```

```kusto
AzureActivity
| where TimeGenerated > ago(24h)
| where OperationNameValue =~ "Microsoft.Storage/storageAccounts/listKeys/action"
| where ActivityStatusValue =~ "Succeeded"
| project TimeGenerated, ResourceGroup, Resource, Caller, CallerIpAddress, ActivityStatusValue
```

---

## Analysis

| Scenario | Query objective | Expected result | ✓ |
|----------|-------------|---------------------|---|
| VM shutdowns in the last 10 minutes | Find stop/deallocate actions | At least one successful VM shutdown is displayed | ✅ |
| New resources via deployments in the last 24 hours | Identify deployment write operations | At least one successful deployment entry appears | ✅ |
| VM creation in the last 24 hours, including creator | Make the VM creator visible | The `Caller` of the creation is included in the result | ✅ |
| Successful listing of storage account keys | Verify sensitive key operation and source IP | `Caller` and `CallerIpAddress` are visible | ✅ |

---

## Notes

- **AzureActivity:** Exported activity logs are stored in the `AzureActivity` table within the Log Analytics workspace.
- **Activity log:** It records management or control plane actions, not primarily data access.
- **Warning ⚠️:** Actions such as `listKeys` are security-relevant because they can indirectly enable access to storage data.

