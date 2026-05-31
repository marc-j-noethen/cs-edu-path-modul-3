# 🖥️ The Portal Creator - Creating and removing a virtual machine in the Azure portal

**Course:** Cloud & Enterprise Security - Intro to Cloud | **Date:** 13 April 2026

---

## Task

**Objective:** Complete the Microsoft Learn lab on creating an Azure resource in the portal, provision a virtual machine, and then delete the resources created.

**Prerequisites:**
- Completion of the Microsoft Learn lab `exercise-create-azure-resource`.
- Creation of a virtual machine via the Azure portal’s graphical interface.
- Submit a screenshot of the virtual machine created and then clean up all the resources created.

---

## Solution

### Creation in the portal

```text
1. Open the Microsoft Learn lab "Create an Azure resource".
2. In the Azure portal, launch the wizard for creating a virtual machine.
3. Apply the settings recommended in the lab.
4. Complete the deployment and wait until the virtual machine has been successfully created.
5. Open the virtual machine’s overview page and take a screenshot.
```

### Clean-up

```text
1. Following the documentation and screenshot, open the associated resource group.
2. Delete the resource group or the virtual machine along with all dependent resources.
3. Check that there are no unnecessary residual resources remaining.
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Create VM in the portal | Familiarise yourself with graphical provisioning in Azure | A new virtual machine is successfully provisioned | ✅ |
| Open VM overview | Visual inspection of the created resource | The name, status and configuration of the VM are visible | ✅ |
| Take a screenshot | Secure evidence for submission | The created virtual machine is visible in the screenshot | ✅ |
| Delete resources | Avoid costs and unnecessary resources | The associated resources are completely removed | ✅ |

---

## Notes

- **Virtual machine:** A VM is a compute-based Azure resource with its own operating system and network components.
- **Resource group:** A resource group bundles related Azure resources for joint management.
- **Warning ⚠️:** Cloud resources that are not deleted may continue to incur costs or create an unnecessary attack surface.

