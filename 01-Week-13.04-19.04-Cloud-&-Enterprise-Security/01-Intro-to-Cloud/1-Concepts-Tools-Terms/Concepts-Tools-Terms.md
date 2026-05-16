# Introduction to the Cloud

## 📊 Summary based on the 80/20 principle

### 1. Cloud computing replaces hardware ownership with usage-based IT
The core of the 80/20 principle is the shift from owning your own infrastructure to using rented services. Three things are crucial: the difference between on-premises and cloud, the deployment models (public/private/hybrid cloud) and the service models (IaaS, PaaS and SaaS).

### 2. Step-by-step core process
1. Determine which resource you need: compute, storage, database or ready-made software.
2. Choose the appropriate service model: IaaS for full control, PaaS for faster development, SaaS for pure usage.
3. Choose the appropriate deployment model: public, private or hybrid cloud.
4. Assign resources in Azure clearly to a region, availability zone and resource group.

### 3. Interactive mode / Using tools
A typical first step in Azure is: sign in, check your subscription and create a resource group. This is exactly how almost every subsequent lab environment begins.

### 4. Key concepts with code examples
- **Public Cloud:** Shared provider infrastructure offering high speed and elasticity.
- **IaaS, PaaS, SaaS:** The key difference lies in who manages the operating system, runtime and application.
- **Region and Availability Zone:** These influence latency, reliability and compliance.
- **Resource Group:** This is the logical container in which Azure resources are managed collectively.

```bash
az login
az account show --output table
az group create --name rg-intro-cloud --location westeurope
```

### 5. Comparison: On-premises vs. Cloud
- On-premises requires CapEx, lead times, operation, cooling and maintenance of your own hardware.
- The cloud utilises OpEx, reduces deployment times from weeks to minutes and scales elastically.
- The cloud is ideal for rapid experimentation; on-premises often offers more physical control.

### 6. Why is this important / Benefits
Once you have understood these basic models, you will be able to grasp later Azure topics such as costs, security, scaling and architecture much more quickly.

**Quick Start Checklist**
- ☐ I can distinguish between CapEx and OpEx.
- ☐ I can explain public, private and hybrid cloud.
- ☐ I know the difference between IaaS, PaaS and SaaS.
- ☐ I know what region, availability zone and resource group mean in Azure.

**Key point**
Cloud does not just mean ‘servers on the internet’, but controlled, scalable IT as a service with clear operational and cost models.

---

## Table 1: Tools used
| Tool                     | Meaning                                           |
| ------------------------ | ------------------------------------------------- |
| Azure Portal             | Web interface for managing cloud resources        |
| Azure CLI                | Command-line tool for Azure operations            |
| Azure Pricing Calculator | Estimates monthly cloud costs prior to deployment |
| Resource Group           | Logical container for related Azure resources     |

## Table 2: Technical terms
| Term | Meaning |
|---|---|
| Cloud computing | Provision of IT resources via the internet on demand |
| CapEx | One-off investment costs for proprietary infrastructure |
| OpEx | Ongoing operating costs in a usage-based model |
| Availability Zone | Physically separate zone within an Azure region |

## Table 3: Key terms
| Term | Meaning |
|---|---|
| pay-as-you-go | pay only for what you use |
| deploy | deploy |
| scale | scale |
| workload | workload |



