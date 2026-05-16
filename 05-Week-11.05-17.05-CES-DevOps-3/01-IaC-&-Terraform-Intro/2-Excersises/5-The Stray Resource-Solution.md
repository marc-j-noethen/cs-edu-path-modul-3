# 🖥️ The Stray Resource - Importing an existing VNet via Terraform

**Course:** CES DevOps 3 - IaC & Terraform Intro | **Date:** 11 May 2026

---

## Task

**Objective:** Import a manually created Azure resource into Terraform without recreating it, and then successfully manage it further.

**Requirements:**
- Manually create a VNet named `legacy-vnet` with the address space `172.16.0.0/16` in the Azure portal.
- Write a suitable Terraform resource block for it.
- Observe the expected error when running `terraform apply`, as the resource already exists.
- Import the Azure resource into the state using `terraform import` and then successfully modify it.

---

## Solution

### Terraform code

```hcl
resource "azurerm_virtual_network" "legacy" {
  name                = "legacy-vnet"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  address_space       = ["172.16.0.0/16"]
}
```

### Import commands

```bash
terraform apply
terraform import azurerm_virtual_network.legacy /subscriptions/<subscription-id>/resourceGroups/myTFResourceGroup/providers/Microsoft.Network/virtualNetworks/legacy-vnet
terraform plan
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| First `terraform apply` | Highlight ownership conflict | Terraform reports that the resource already exists and has not yet been imported. | ✅ |
| `terraform import ...` | Add existing resource to the state | Terraform subsequently recognises the manual VNet as a managed object. | ✅ |
| `terraform plan` after import | Validate alignment | If the configuration is identical, no further changes are shown. | ✅ |
| Change address space later | Demonstrate administrative control | After the import, the resource can be modified normally via code. | ✅ |

---

## Notes

- **Import:** `terraform import` adds an existing resource to the state; it does not create a new one.
- **Resource ID:** The full Azure resource ID is mandatory for the import.
- **Legacy migration:** This is exactly how legacy portal resources are gradually migrated to Infrastructure as Code.
