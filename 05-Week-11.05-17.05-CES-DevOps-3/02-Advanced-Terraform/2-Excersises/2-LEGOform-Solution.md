# 🖥️ LEGOform - Combining the local module and the registry module

**Course:** CES DevOps 3 - Advanced Terraform | **Date:** 12 May 2026

---

## Task

**Objective:** To build a modular Terraform structure in which a custom storage module and the current Azure Verified VNet module from the registry work together.

**Requirements:**
- Create a local module `modules/storage_account`.
- In `main.tf`, also use the current AVM VNet module from the Terraform Registry.
- Call the local module twice and define outputs for the VNet ID and blob endpoints.
- Set up the development environment using `dev.tfvars` with `-var-file`.

---

## Solution

### Local module

```hcl
# modules/storage_account/main.tf
resource "azurerm_storage_account" "this" {
  name                     = var.account_name
  location                 = var.location
  resource_group_name      = var.resource_group_name
  account_tier             = var.account_tier
  account_replication_type = "LRS"
}

output "primary_blob_endpoint" {
  value = azurerm_storage_account.this.primary_blob_endpoint
}
```

### Root configuration

```hcl
module "vnet" {
  source  = "Azure/avm-res-network-virtualnetwork/azurerm"
  version = "0.17.1"

  name          = "ces-dev-vnet"
  location      = azurerm_resource_group.rg.location
  parent_id     = azurerm_resource_group.rg.id
  address_space = ["10.20.0.0/16"]
}

module "logging_storage" {
  source              = "./modules/storage_account"
  account_name        = "lklogging4721"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  account_tier        = var.storage_tier
}

module "data_storage" {
  source              = "./modules/storage_account"
  account_name        = "lkdata4721"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  account_tier        = var.storage_tier
}

output "vnet_resource_id" {
  value = module.vnet.resource_id
}

output "blob_endpoints" {
  value = {
    logging = module.logging_storage.primary_blob_endpoint
    data    = module.data_storage.primary_blob_endpoint
  }
}
```

### Variables and Apply

```bash
mkdir -p configs
printf 'storage_tier = "Standard"\n' > configs/dev.tfvars
printf 'storage_tier = "Premium"\n' > configs/prod.tfvars

terraform init
terraform apply -var-file=configs/dev.tfvars
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| Local module | Encapsulate reusable custom logic | A storage blueprint can be used multiple times without copy-pasting. | ✅ |
| AVM-VNet module `0.17.1` | Integrate the latest community/partner module | The VNet is provisioned via the official Azure Verified Module. | ✅ |
| Bundle outputs | Make important target values transparent | The VNet ID and both blob endpoints appear grouped together after the apply. | ✅ |
| `-var-file=configs/dev.tfvars` | Select environment specifically | The development variant uses Standard tier instead of Premium. | ✅ |

---

## Notes

- **Modules:** In Terraform, modules are the central technique for breaking down infrastructure in a logical and reusable way.
- **AVM module:** The current registry module now uses `parent_id` instead of `resource_group_name`.
- **Multiple calls:** The two storage instances clearly demonstrate why modular configurations scale well in teams.

