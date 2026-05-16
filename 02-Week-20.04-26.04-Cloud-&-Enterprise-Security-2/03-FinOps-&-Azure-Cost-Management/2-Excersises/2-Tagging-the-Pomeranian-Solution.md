# 🖥️ Tagging the Pomeranian - Applying cost tags to resources

**Course:** Cloud & Enterprise Security 2 - FinOps & Azure Cost Management | **Date:** 22 April 2026

---

## Task

**Objective:** Set up a small infrastructure and ensure that the resource group, public IP and VNet are assigned the same cost tags.

**Requirements:**
- Create the resource group `simba-app-rg`.
- Provision a simple VNet and a public IP within this resource group.
- Assign the tags `Project: Simba` and `Environment: Development` at group and resource level.

---

## Solution

### Portal steps

```text
1. In the Azure portal, create a resource group named simba-app-rg.
2. Deploy a simple public IP and a VNet within this resource group.
3. Set the following tags on the resource group:
   - Project = Simba
   - Environment = Development
4. Apply the same tags manually to the public IP.
5. Apply the same tags manually to the VNet.
6. Open the resource group overview page and take a screenshot showing the resources and tags.
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Create resource group `simba-app-rg` | Prepare project container | The group exists as a shared scope | ✅ |
| Create public IP and VNet | Create example infrastructure for FinOps tagging | Both resources are in the same group | ✅ |
| Set tags on resource group | Identify project and environment | The group carries both metadata | ✅ |
| Set tags manually on resources | Ensure cost allocation at resource level | Public IP and VNet show the same tags | ✅ |

---

## Notes

- **Tags and inheritance:** Tags from a resource group are not automatically inherited by the resources it contains.
- **Cost allocation:** Consistent tags simplify analysis by project or environment.
- **Metadata strategy:** Even small environments benefit from consistent and early tagging discipline.

