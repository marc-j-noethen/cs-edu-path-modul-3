# 🖥️ Structured Infrastructure - Run the HashiCorp Azure basic workflow up to `destroy`

**Course:** CES DevOps 3 - IaC & Terraform Intro | **Date:** 11 May 2026

---

## Task

**Objective:** Work through the official Azure Terraform tutorial using the latest provider block and follow the complete lifecycle from `init` to `destroy`.

**Prerequisites:**
- Install Terraform and set up a working directory.
- Use the `main.tf` file provided by the course with the latest provider block for Azure.
- Run `terraform init`, `plan`, `apply`, and then make a small change followed by another `apply`.
- Finally, remove the infrastructure with `terraform destroy` and verify that `Destroy complete!` is displayed.

---

## Solution

### `main.tf`

```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "4.57.0"
    }
  }
  required_version = ">= 1.14.3"
}

provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "rg" {
  name     = "myTFResourceGroup"
  location = "germanywestcentral"
}
```

### Terraform commands

```bash
terraform init
terraform plan
terraform apply

# then make a small code change
terraform plan
terraform apply
terraform destroy
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `terraform init` | Prepare provider and working directory | Terraform loads the AzureRM provider and initialises the state. | ✅ |
| First `apply` | Deploy base resources | The resource group is created in `germanywestcentral`. | ✅ |
| Second change + `apply` | Practise declarative change flow | Terraform detects the code deviation and implements it in a reproducible manner. | ✅ |
| `terraform destroy` | Clean up and avoid costs | The message `Destroy complete!` confirms the clean rollback. | ✅ |

---

## Notes

- **Terraform lifecycle:** The core idea is: declare, plan, apply, change, clean up.
- **Provider version:** The course file deliberately specifies a current AzureRM version so that the tutorial runs smoothly with newer versions of Terraform.
- **Region slug:** For `Germany West Central`, the slug `germanywestcentral` must be used in Terraform.

