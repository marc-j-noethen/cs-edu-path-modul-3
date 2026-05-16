# FinOps & Azure Cost Management

## 📊 Summary based on the 80/20 principle

### 1. Cost control is also security control
The essence of the 80/20 principle is this: the cloud is billed as OpEx rather than CapEx, and it is precisely this flexibility that can spiral out of control without proper governance. FinOps combines technology, finance and security to make costs, responsibilities and anomalies transparent.

### 2. Step-by-step core process
1. Organise resources clearly into subscriptions and resource groups.
2. Enforce tags such as Owner, Department or Environment.
3. Set up budgets and alerts, rather than just reviewing invoices after the fact.
4. Always investigate cost increases as potential security incidents.

### 3. Interactive Mode / Tool Usage
The quickest practical lever is tagging. Only what is properly tagged can be meaningfully analysed for costs, accountability and anomalies.

### 4. Key Concepts with Code Examples
- **FinOps:** Operating model for financial accountability in the cloud.
- **Tags:** Name-value pairs that make cost centres, environments or owners visible.
- **Budgets and Alerts:** Do not automatically stop resources, but provide early warning signals.
- **Security Overlap:** Crypto mining, zombie resources or oversized systems often first appear in cost patterns.

```bash
az resource tag \
  --ids /subscriptions/<subscription-id>/resourceGroups/rg-finops \
  --tags Department=Security Environment=Dev Owner=Team-Blue
```

### 5. Comparison: CapEx vs. OpEx
- CapEx involves a high upfront investment in proprietary hardware.
- OpEx involves ongoing, variable usage costs in the cloud.
- OpEx is more flexible, but requires stronger monitoring and governance.

### 6. Why is this important / Benefits
FinOps does more than just protect the budget. It also reduces shadow resources, establishes accountability and often provides very early indications of misuse in the cloud.

**Quick Start Checklist**
- ☐ I can explain CapEx and OpEx.
- ☐ I know why tags are important for cost reporting.
- ☐ I understand the purpose of budgets and alerts.
- ☐ I understand why cost anomalies can be a security signal.

**Key point**
In the cloud, an unexplained bill is often not just a financial problem, but a potential incident.

---

## Table 1: Tools used
| Tool | Purpose |
|---|---|
| Azure Cost Management | Monitors and analyses cloud expenditure |
| Azure Pricing Calculator | Simulates costs prior to deployment |
| Tags | Assign costs and responsibilities |
| Budget Alerts | Warn when budget thresholds are reached, rather than only when the bill arrives |

## Table 2: Technical terms
| Term | Meaning |
|---|---|
| FinOps | Framework for cost accountability in the cloud |
| Budget | Defined spending limit for a scope or period |
| Tagging | Labelling resources with metadata |
| Right-Sizing | Sizing resources to match the actual load |

## Table 3: Key Vocabulary
| Term | Meaning |
|---|---|
| spending | Expenditure |
| alert | Warning message |
| owner | Responsible person or team |
| idle | Unused or inactive |


