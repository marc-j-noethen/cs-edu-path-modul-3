# 🖥️ The Crystal Ball – Estimating monthly Azure costs using the pricing calculator

**Course:** Cloud & Enterprise Security 2 – FinOps & Azure Cost Management | **Date:** 22 April 2026

---

## Task

**Objective:** Create a monthly cost estimate for a defined Azure architecture in the pricing calculator.

**Requirements:**
- Use the Azure Pricing Calculator with the `West Europe` region.
- Configure two `B2s` Linux VMs, a serverless Azure SQL Single Database, and a Blob storage account with 500 GB and approximately 10,000 write operations.
- Submit a screenshot showing the individual resources and the total monthly cost.

---

## Solution

### Pricing Calculator Configuration

```text
1. Open the Azure Pricing Calculator.
2. Add two virtual machine entries:
   - Operating system: Ubuntu Linux
   - Size: B2s
   - Runtime: 730 hours/month
   - Region: West Europe
   - Quantity: 2
3. Add Azure SQL Database:
   - Purchase model: Single Database, vCore
   - Tier: General Purpose
   - Compute: Serverless
   - vCores: 2
   - Storage: 10 GB
   - Region: West Europe
4. Add storage account:
   - Blob Storage / Block Blob
   - Performance: Standard
   - Redundancy: LRS
   - Capacity: 500 GB/month
   - Write operations: approx. 10,000
   - Region: West Europe
5. Check the total and take a screenshot.
```

---

## Analysis

| Resource | Configuration | Expected result | ✓ |
|-----------|---------------|---------------------|---|
| VMs | 2x Ubuntu Linux, `B2s`, 730 h, West Europe | Two cost-effective compute entries in the calculator | ✅ |
| Azure SQL Database | Single DB, vCore, General Purpose, Serverless, 2 vCores, 10 GB | A matching database cost entry | ✅ |
| Storage | Block Blob, Standard, LRS, 500 GB, 10,000 writes | A storage entry with usage values | ✅ |
| Total | All components added together | Monthly estimate is visible | ✅ |

---

## Notes

- **Price calculator:** The Azure Pricing Calculator is intended for preliminary estimates, not for exact subsequent invoice amounts.
- **Region:** Prices vary depending on the Azure region, so `West Europe` must be used consistently.
- **FinOps:** Early cost estimates help to better align technical architecture and budget approvals.

