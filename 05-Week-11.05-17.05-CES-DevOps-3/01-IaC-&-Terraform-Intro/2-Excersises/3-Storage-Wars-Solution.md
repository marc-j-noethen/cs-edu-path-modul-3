# 🖥️ Storage Wars – Provision an Azure Storage Account using Terraform and retrieve the key

**Course:** CES DevOps 3 – IaC & Terraform Intro | **Date:** 11 May 2026

---

## Task

**Objective:** Provision a storage account with the required Azure properties declaratively and read the `primary_access_key` from Terraform without accessing the portal.

**Requirements:**
- Add an `azurerm_storage_account` to `main.tf`.
- Use the `Standard_LRS` SKU and set a globally unique name.
- Deploy the resource using Terraform.
- Make the key visible via a Terraform output or state query in the terminal.

---

## Solution

### Terraform code

```hcl
resource "azurerm_storage_account" "lab" {
  name                     = "lkstorage4721"
  resource_group_name      = azurerm_resource_group.rg.name
  location                 = azurerm_resource_group.rg.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}

output "storage_primary_access_key" {
  value     = azurerm_storage_account.lab.primary_access_key
  sensitive = true
}
```

### Terraform commands

```bash
terraform apply
terraform output -raw storage_primary_access_key
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `azurerm_storage_account` | Create Azure Storage declaratively | The account is created with Standard tier and LRS replication. | ✅ |
| Globally unique name | Comply with Azure naming rules | The resource name is globally available and accepted. | ✅ |
| `terraform output -raw ...` | Retrieve sensitive value without using the portal | The `primary_access_key` appears directly in the terminal. | ✅ |

---

## Notes

- **Storage Account:** Azure requires a globally unique name in lowercase for storage accounts.
- **Sensitive Output:** With `sensitive = true`, the value remains masked in standard output; `-raw` displays it explicitly.
- **Practical relevance:** Cleanly reading secrets from IaC without having to jump to the portal is a key workflow.

