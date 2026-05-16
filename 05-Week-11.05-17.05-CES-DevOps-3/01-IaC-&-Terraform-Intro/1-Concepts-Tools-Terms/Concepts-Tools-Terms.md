# IaC & Terraform Intro

## 📊 Summary based on the 80/20 principle

### 1. Infrastructure as Code makes infrastructure reproducible and versionable
The essence of the 80/20 principle is this: instead of building resources with a click, you describe the desired state in code. Terraform reads this declarative state, compares it with reality, and implements changes in a controlled manner.

### 2. Step-by-step core process
1. Write infrastructure in HCL as a declarative definition.
2. Initialise the provider and backend with `terraform init`.
3. Check the change plan with `terraform plan`.
4. Implement the desired state with `terraform apply` and protect the state file.

### 3. Interactive mode / Using the tool
In practice, the process almost always revolves around the same lifecycle: init, plan, apply, and possibly destroy later. If you understand this process, you understand Terraform.

### 4. Key concepts with code examples
- **Declarative rather than imperative:** You describe the end state, not every individual step.
- **Providers:** Translate Terraform code into API calls, for example for AzureRM.
- **State file:** Maps code to real-world resources and often contains sensitive information.
- **Bicep and ARM:** Azure-native alternatives, whilst Terraform works across clouds.

```hcl
resource "azurerm_resource_group" "example" {
  name     = "my-first-tf-rg"
  location = "West Europe"
}
```

### 5. Comparison: Declarative vs. imperative
- Imperative describes every single step required to achieve the goal.
- Declarative describes the target state; the tool calculates the path.
- Terraform is primarily declarative and therefore better suited to repeatable infrastructure.

### 6. Why is this important / Benefits
IaC reduces configuration drift, manual errors and a lack of traceability. This is precisely why Terraform is a key tool for modern cloud and security work.

**Quick-start checklist**
- ☐ I can explain IaC in one sentence.
- ☐ I am familiar with the Terraform lifecycle.
- ☐ I know why the state file must be protected.
- ☐ I can distinguish between declarative and imperative approaches.

**Key point**
Terraform replaces click-through paths with versionable infrastructure states, making cloud deployment repeatable, verifiable and more secure.

---

## Table 1: Tools used
| Tool | Description |
|---|---|
| Terraform | IaC tool for declarative infrastructure |
| AzureRM Provider | Terraform provider for Azure resources |
| VS Code Terraform Extension | Facilitates writing HCL |
| Azure Bicep | Azure-native alternative for Infrastructure as Code |

## Table 2: Technical terms
| Term | Meaning |
|---|---|
| HCL | HashiCorp Configuration Language for Terraform |
| Provider | Interface between Terraform and the target platform |
| State | Mapping file between code and actual resources |
| Drift | Deviation between desired and actual state |

## Table 3: Key terms
| Term | Meaning |
|---|---|
| apply | apply |
| plan | Execution plan |
| destroy | delete |
| backend | Storage location for state data |


