# 🖥️ Stashed Away - Migrating Remote State to Azure Storage and Testing Locking

**Course:** CES DevOps 3 - Advanced Terraform | **Date:** 12 May 2026

---

## Task

**Objective:** Move the Terraform state from the local file to an Azure Storage backend and demonstrate the locking mechanism in practice.

**Requirements:**
- Prepare an Azure Storage backend in accordance with the Microsoft workflow.
- Enter the `backend "azurerm"` block and migrate the state using `terraform init`.
- Test the lock using two terminal windows.
- Additionally, summarise in one sentence the added value of this Azure-native backend compared to HCP Terraform.

---

## Solution

### Backend block

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "tfstate-rg"
    storage_account_name = "lkterraformstate01"
    container_name       = "tfstate"
    key                  = "advanced/lab.tfstate"
  }
}
```

### Test commands

```bash
terraform init

# Terminal A
terraform apply

# Terminal B, whilst A waits for yes/no
terraform plan
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `terraform init` after backend block | Trigger state migration | Terraform offers to move the existing local state to the Azure backend. | ✅ |
| Open `terraform apply` in Terminal A | Create lock situation | The state is reserved exclusively for the current operation. | ✅ |
| `terraform plan` in Terminal B | Validate lock | Terraform reports that the state is currently locked and cannot be edited in parallel. | ✅ |

---

## Notes

- **State Lock:** The lock prevents concurrent changes and thus corrupt or inconsistent state files.
- **Azure Backend Advantage:** An Azure-native backend often remains simpler within the same cloud and access boundaries of an Azure-centric team.
- **Comparison with HCP Terraform:** HCP offers greater collaboration and governance, whereas Azure Storage provides maximum proximity to the Azure platform and often simpler data sovereignty.

