# 🖥️ No Tag, No Bag – Defining Terraform compliance guardrails and getting the green light

**Course:** CES DevOps 3 – Advanced Terraform | **Date:** 12 May 2026

---

## Task

**Objective:** Enforce two compliance rules prior to deployment: Mandatory `Department` tag and disabled public network access for storage accounts.

**Requirements:**
- Create a storage account that deliberately violates the policy.
- Create a `policy/security.feature` file for terraform-compliance.
- Generate a plan, convert it to JSON and check it against the feature file.
- Then correct the code and pass the test.

---

## Solution

### Unsafe initial state

```hcl
resource "azurerm_storage_account" "bad_example" {
  name                          = "lkbadexample4721"
  resource_group_name           = azurerm_resource_group.rg.name
  location                      = azurerm_resource_group.rg.location
  account_tier                  = "Standard"
  account_replication_type      = "LRS"
  public_network_access_enabled = true
}
```

### Feature file (`policy/security.feature`)

```gherkin
Feature: Storage account security guardrails

Scenario: Ensure all taggable resources have tags
  Given I have resource that supports tags defined
  Then it must contain tags
  And its value must not be null

Scenario: Ensure all taggable resources include the Department tag
  Given I have resource that supports tags defined
  When it has tags
  Then it must contain tags
  Then it must contain Department
  And its value must match ".+" regex

Scenario: Ensure storage accounts disable public network access
  Given I have azurerm_storage_account defined
  Then it must contain public_network_access_enabled
  And its value must be false
```

### Testing and fixing procedure

```bash
terraform plan -out=plan.out
terraform show -json plan.out > plan.json
terraform-compliance -f policy -p plan.json

# then correct main.tf:
# - tags = { Department = "Security" }
# - public_network_access_enabled = false
```

---

## Analysis

| Step/Command | Purpose | Expected Result | ✓ |
|---|---|---|---|
| Policy file | Express guardrails as BDD rules | Compliance requirements are versioned in a readable and testable format. | ✅ |
| First run against the rogue code | Deliberately highlight errors | terraform-compliance reports the missing tags and the open network switch. | ✅ |
| Code fix | Comply with policy | After adding tags plus `public_network_access_enabled = false`, the run turns green. | ✅ |

---

## Notes

- **terraform-compliance:** The tool tests Terraform plans against technical or security-related rules prior to deployment.
- **BDD syntax:** Map-like attributes such as `tags` should be checked key-by-key, which is why the scenario first drills into `tags` and then asserts the `Department` key.
- **Pre-deployment check:** It is precisely these kinds of guardrails that prevent insecure infrastructure from ever landing in Azure.
