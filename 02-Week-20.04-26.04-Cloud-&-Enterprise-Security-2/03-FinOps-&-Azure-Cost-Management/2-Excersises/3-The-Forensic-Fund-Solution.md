# 🖥️ The Forensic Fund - Set up a budget with a forecast alert for tagged resources

**Course:** Cloud & Enterprise Security 2 - FinOps & Azure Cost Management | **Date:** 22 April 2026

---

## Task

**Objective:** Configure a monthly Azure budget for resources tagged with `Project: MalwareSandbox` and set up an email alert when forecast costs reach 90%.

**Requirements:**
- Budget amount of `€50` per month.
- Filter the scope so that only resources with `Project: MalwareSandbox` are included.
- Set up an alert based on the *forecasted cost* at `90%`, with an email sent to the student’s address.

---

## Solution

### Portal steps

```text
1. If necessary, temporarily create a dummy resource with the tag Project = MalwareSandbox.
2. In Azure Cost Management, switch to the appropriate scope, for example Subscription or Resource Group.
3. Open "Budgets" and create a new budget.
4. Set filters so that only costs with the tag Project = MalwareSandbox are included.
5. Set the monthly budget to 50 EUR.
6. Configure an alert:
   - Type: Forecasted cost
   - Threshold: 90%
   - Notification: Email to the student’s address
7. Open the "Review + create" screen and take a screenshot.
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Set tag-based filtering | Limit the budget to the specific malware sandbox project | Only relevant resources are included in the budget | ✅ |
| Set monthly budget to `€50` | Define a strict cost ceiling | The budget displays €50 as the limit | ✅ |
| Forecast warning at `90%` | Enable early response before actual overspend | Warning triggers as soon as the forecast approaches the limit | ✅ |
| Enter email notification | Ensure proactive information | The student’s email address is stored as the recipient | ✅ |

---

## Notes

- **Forecasted Cost:** The warning reacts to a cost forecast, not to actual costs that have already been incurred.
- **Budget filter:** Budgets can be filtered by tags, resources or other properties.
- **Warning ⚠️:** Budgets do not automatically stop resources; they provide information so that people or automations can react.

