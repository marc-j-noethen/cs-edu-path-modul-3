# Advanced Terraform

## 📊 Summary based on the 80/20 principle

### 1. Production-ready Terraform relies on modules, remote state and policy-as-code
The 80/20 core of advanced Terraform is no longer just about creating a resource, but about scaling the working model cleanly. This includes variables and outputs, local and registry modules, remote backends with locking, and automated compliance checks before applying.

### 2. Step-by-step core process
1. Parameterise configurations with variables, `tfvars` and outputs.
2. Break down infrastructure into local modules and reusable registry modules.
3. Move the state to a remote backend or HCP Terraform for collaboration and locking.
4. Validate plans before applying using policy-as-code, for example via `terraform-compliance`.

### 3. Interactive mode / Tool usage
In practice, you’ll work at an advanced level with `plan.out`, JSON output and backends. Only then does Terraform become team-ready, auditable and truly manageable for larger labs such as GOAD.

### 4. Key concepts with code examples
- **Modules:** Package reusable infrastructure logic, either locally or from the Terraform Registry.
- **Remote State:** Prevents the proliferation of local `tfstate` files and enables state locking.
- **HCP Terraform or Azure Storage Backend:** Both are professional approaches to shared state.
- **Policy-as-Code:** Guardrails such as mandatory tags or prohibited public access are automatically checked before deployment.

```hcl
terraform {
  backend "azurerm" {}
}

module "storage_logging" {
  source       = "./modules/storage_account"
  account_name = "stlogging01"
  location     = "westeurope"
}
```

### 5. Comparison: Local State vs. Remote State
- Local State is quick for demos, but poor for collaboration, backups and security.
- Remote state enables locking, sharing and controlled storage of sensitive data.
- For teams or production-ready environments, remote state is practically a must.

### 6. Why is this important / Benefits
Advanced Terraform is the point at which a learning tool becomes a genuine platform for teamwork, governance and large-scale cloud deployment.

**Quick Start Checklist**
- ☐ I can explain why modules are important.
- ☐ I know the difference between local and remote state.
- ☐ I know what `plan.out` and `terraform show -json` are useful for.
- ☐ I understand the purpose of Policy-as-Code before applying.

**Key takeaway**
It is only with modules, remote state and automated guardrails that Terraform moves from ‘works locally’ to ‘works in a team’.

---

## Table 1: Tools used
| Tool | Description |
|---|---|
| Terraform Registry | Source of reusable community modules |
| HCP Terraform | Managed platform for workspaces and remote state |
| Azure Storage Backend | Azure-native storage location for Terraform state |
| terraform-compliance | Policy-as-code validation based on Terraform plans |

## Table 2: Technical terms
| Term | Meaning |
|---|---|
| Modules | Reusable Terraform building blocks |
| Remote Backend | External storage location for the Terraform state |
| State Locking | Locks the state against concurrent changes |
| Policy-as-Code | Automated policy checking prior to deployment |

## Table 3: Key terms
| Term | Meaning |
|---|---|
| workspace | Workspace |
| output | Output value |
| backend | State storage |
| compliance | Rule compliance |


