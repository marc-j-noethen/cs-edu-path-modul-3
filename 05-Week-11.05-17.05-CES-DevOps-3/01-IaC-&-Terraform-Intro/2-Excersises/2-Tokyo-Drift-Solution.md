# 🖥️ Tokyo Drift – Detecting Terraform drift and restoring the desired state

**Course:** CES DevOps 3 – IaC & Terraform Intro | **Date:** 11 May 2026

---

## Task

**Objective:** Identify manual changes in the Azure portal as drift and use Terraform to restore the environment to the desired state.

**Requirements:**
- Continue using the environment from the previous exercise.
- Manually create a subnet in the Azure portal and modify tags on the VNet.
- Then run `terraform plan` and `terraform apply` locally.
- Save the plan output as evidence of the drift detection.

---

## Solution

### Terraform commands

```bash
terraform plan
terraform apply
```

### Interpretation

```text
Terraform compares the last known state with the actual Azure environment.
Manually modified tags are displayed in the plan as rollbacks to the declared configuration.
Additional network changes not specified in the code are also visible as deviations,
as soon as they affect the managed resource.
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| Manual portal changes | Deliberately create drift | The Azure reality deviates from the Terraform code. | ✅ |
| `terraform plan` | Identify deviations | Terraform shows which properties would be reset or adjusted. | ✅ |
| `terraform apply` | Restore desired state | The environment then corresponds to the declared configuration again. | ✅ |

---

## Notes

- **Drift:** Drift occurs when real cloud resources are modified outside of Terraform.
- **State file:** The Terraform state serves as the basis for comparison between the code and the actual infrastructure.
- **Key takeaway:** Especially in teams, this exercise demonstrates why manual portal changes are problematic in the long term.

