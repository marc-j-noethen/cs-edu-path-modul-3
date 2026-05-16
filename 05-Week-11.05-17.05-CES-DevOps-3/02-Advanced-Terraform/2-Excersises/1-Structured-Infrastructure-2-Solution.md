# 🖥️ Structured Infrastructure 2 - Setting up variables, outputs and HCP Terraform

**Course:** CES DevOps 3 - Advanced Terraform | **Date:** 12 May 2026

---

## Task

**Objective:** Extend the beginner’s project to use variables, outputs and a healthy HCP Terraform workspace for the remote state.

**Requirements:**
- Parameterise the resource group name using a variable.
- Define an output variable for the resource group ID.
- Migrate the state to an HCP Terraform workspace named `learn-terraform-azure`.
- Verify in the dashboard that the workspace is healthy and the migration was successful.

---

## Solution

### Terraform configuration

```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "4.57.0"
    }
  }

  cloud {
    organization = "<your-hcp-organisation>"
    workspaces {
      name = "learn-terraform-azure"
    }
  }
}

variable "resource_group_name" {
  type    = string
  default = "myTFResourceGroup"
}

resource "azurerm_resource_group" "rg" {
  name     = var.resource_group_name
  location = "germanywestcentral"
}

output "resource_group_id" {
  value = azurerm_resource_group.rg.id
}
```

### Terraform commands

```bash
terraform login
terraform init
terraform apply
terraform output resource_group_id
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| Variable `resource_group_name` | Make configuration more flexible | The resource group name is no longer hard-coded. | ✅ |
| Output `resource_group_id` | Export important information | The ID can be viewed directly in the terminal and in HCP Terraform. | ✅ |
| `cloud { ... }` block | Move state to HCP Terraform | The workspace takes over remote state management. | ✅ |

---

## Notes

- **HCP Terraform:** The platform was formerly known as Terraform Cloud and centrally manages runs, state and collaboration.
- **`terraform login`:** This allows the local CLI to authenticate against HCP Terraform once.
- **Healthy Workspace:** This status confirms that the state, run history and workspace configuration are functioning correctly.

