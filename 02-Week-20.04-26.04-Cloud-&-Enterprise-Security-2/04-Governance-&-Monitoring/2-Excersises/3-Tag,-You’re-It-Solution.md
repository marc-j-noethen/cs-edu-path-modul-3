# 🖥️ Tag, You’re It – Enforcing and automatically correcting tagging rules with Azure Policy

**Course:** Cloud & Enterprise Security 2 – Governance & Monitoring | **Date:** 23 April 2026

---

## Task

**Objective:** First, strictly enforce a tagging rule for `Cost Centre`, then modify it so that missing tags are automatically added and existing resources are corrected via remediation.

**Requirements:**
- Create a new resource group with the tag `Cost Centre = 000`.
- Assign a policy that blocks resources without a `Cost Centre`.
- Replace this with a policy that automatically adds the tag and performs remediation for non-compliant resources.

---

## Solution

### Portal steps

```text
1. Create a new resource group and tag it with Cost Centre = 000.
2. Assign a policy that requires the Cost Centre tag on resources, for example "Require a tag on resources".
3. Attempt to create a new resource without this tag.
4. Verify that the provisioning is blocked and take a screenshot of the error message.
5. Remove the first policy.
6. Assign a second policy that automatically adds the tag, for example, "Inherit a tag from the resource group if missing".
7. Enable remediation so that existing non-compliant resources can be corrected.
8. Create a new resource without a manually set tag.
9. Verify that the Cost Centre tag appears automatically on the resource.
10. Take a screenshot of the successfully created resource with the automatically added tag.
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Tag resource group with `Cost Centre = 000` | Set reference value for the exercise | The group carries the desired cost centre | ✅ |
| Assign the `Require a tag on resources` policy | Strictly prevent missing tags initially | Resources without a tag cannot be provisioned | ✅ |
| Replace the policy with an inheritance/modify policy | Make governance more user-friendly | Missing tags are automatically added | ✅ |
| Enable remediation | Bring existing non-compliant resources into line | Older resources can also be corrected | ✅ |
| Create a new resource without a tag | Verify automatically | The `Cost Centre` tag appears automatically | ✅ |

---

## Notes

- **Azure Policy:** Policies can either block resources or automatically adjust them using `modify`/inheritance.
- **Tag compliance:** Tag rules are important for cost allocation, inventory management and governance.
- **Warning ⚠️:** An overly strict deny policy can unexpectedly halt deployments if teams are unaware of the tagging rules.

