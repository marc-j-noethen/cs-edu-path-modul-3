# Governance & Monitoring

## 📊 Summary based on the 80/20 principle

### 1. Governance sets guidelines; monitoring identifies deviations
The essence of the 80/20 principle is this: Azure Policy and Resource Locks restrict risky changes before or during deployment. Azure Monitor, Alerts, Service Health and Advisor then provide visibility and recommendations for action.

### 2. Step-by-step core process
1. Define rules with Azure Policy, for example permitted regions or mandatory tags.
2. Protect critical resources with CanNotDelete or ReadOnly locks.
3. Collect metrics and logs centrally via Azure Monitor.
4. Use Alerts, Health Services and Advisor to proactively identify issues.

### 3. Interactive mode / Tool usage
Effective cloud operations combine prevention and monitoring. First guardrails, then telemetry, then alerts and optimisation recommendations.

### 4. Key concepts with code examples
- **Azure Policy:** Enforces rules such as location requirements, SKU limits or tagging obligations.
- **Resource Locks:** Prevent accidental deletion or unintended changes.
- **Azure Monitor:** Uses metrics for trends and logs for events.
- **Azure Advisor:** Provides recommendations on reliability, security, performance, costs and operational excellence.

```bash
az lock create \
  --name protect-rg \
  --lock-type CanNotDelete \
  --resource-group rg-production
```

### 5. Comparison: Resource Health vs. Service Health
- Resource Health shows the status of your specific resource, such as a VM or database.
- Service Health highlights disruptions or maintenance at the Azure service or region level.
- Together, they help distinguish between user error and provider issues.

### 6. Why is this important / Benefits
Without governance, the cloud grows chaotically. Without monitoring, you often only notice outages, attacks or misconfigurations once users are already experiencing them.

**Quick-start checklist**
- ☐ I know what Azure Policy enforces.
- ☐ I can distinguish between CanNotDelete and ReadOnly.
- ☐ I know the difference between metrics and logs.
- ☐ I know what Advisor and Health services are used for.

**Key point**
Governance prevents bad decisions; monitoring makes good operational decisions possible in the first place.

---

## Table 1: Tools used
| Tool | Purpose |
|---|---|
| Azure Policy | Enforces compliance rules for resources |
| Azure Monitor | Collects metrics, logs and alert statuses |
| Azure Service Health | Displays provider-side outages and maintenance |
| Azure Advisor | Recommends improvements for operations and security |

## Table 2: Technical Terms
| Term | Meaning |
|---|---|
| Policy | Set of rules for enforcing standards |
| Resource Lock | Lock preventing deletion or modification |
| Metric | Measurement of a system at a specific point in time |
| Log | Event-based data record for analysis |

## Table 3: Key terms
| Term | Meaning |
|---|---|
| guardrail | Safety barrier |
| compliance | Regulatory compliance |
| alert | Notification |
| outage | Service disruption |

