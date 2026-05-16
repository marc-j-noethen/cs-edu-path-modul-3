# 🖥️ The Rogue Terraform - Detecting hidden VMs outside the default region using Resource Graph

**Course:** Cloud & Enterprise Security 2 - FinOps & Azure Cost Management | **Date:** 22 April 2026

---

## Task

**Objective:** Simulate a suspicious VM in `eastus` and then identify it as an anomaly from the default region `westeurope` using an Azure Resource Graph KQL query.

**Requirements:**
- Deploy a test VM in `eastus`.
- Create a KQL query in the Azure Resource Graph Explorer.
- Output only the resource name, location and tags for all VMs outside of `westeurope`.

---

## Solution

### KQL query

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| where tolower(location) != 'westeurope'
| project name, location, tags
```

### Procedure

```text
1. First, deploy a single test VM in the eastus region.
2. Open the Resource Graph Explorer in the Azure portal.
3. Paste and run the KQL query.
4. Check that the VM deployed in eastus appears in the results.
5. Take a full-screen screenshot showing the query and the results.
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Create test VM in `eastus` | Simulate a suspected external location | A VM exists outside the default region | ✅ |
| Filter `Resources` for VMs | Consider only relevant resource types | Other resource types are excluded | ✅ |
| `tolower(location) != 'westeurope'` | Highlight regional discrepancies | Only unauthorised locations appear | ✅ |
| `project name, location, tags` | Reduce results to identifying data | Name, location and tags are clearly visible | ✅ |

---

## Notes

- **Azure Resource Graph:** A fast, subscription-wide query layer for inventory and governance.
- **KQL:** The Kusto Query Language is well suited for security and compliance searches across many resources.
- **FinOps and Security:** Unplanned resources in foreign regions are often indicators of both cost and security issues.


