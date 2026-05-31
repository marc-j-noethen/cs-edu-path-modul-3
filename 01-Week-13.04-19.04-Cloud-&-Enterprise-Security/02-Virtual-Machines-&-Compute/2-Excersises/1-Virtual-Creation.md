# 🖥️ Virtual Creation - Deploying a Windows VM with IIS in the Portal

**Course:** Cloud & Enterprise Security - Virtual Machines & Compute | **Date:** 14 April 2026

---

## Task

**Objective:** Create a Windows VM via the Azure portal, install IIS as a web server, enable HTTP access, and access the IIS homepage via the public IP address.

**Requirements:**
- Follow the Microsoft Quickstart guide to create a Windows VM in the Azure portal.
- Select the image `Windows Server 2022 Datacenter: Azure Edition - x64 Gen 2`, install IIS and open HTTP port `80`.
- Submit a screenshot of the successfully loaded IIS welcome page and then delete the resource group.

---

## Solution

### Portal steps

```text
1. Search for "Virtual machines" in the Azure portal and select "Create" > "Virtual machine".
2. Create a new VM and select the "Windows Server 2022 Datacenter: Azure Edition - x64 Gen 2" image.
3. Set up an administrator user and a secure password.
4. Under "Inbound port rules", open the RDP (3389) and HTTP (80) ports.
5. Create the VM and connect via RDP once it has been provisioned.
6. Install the IIS web server in PowerShell on the VM:
   Install-WindowsFeature -name Web-Server -IncludeManagementTools
7. Back in the portal, copy the VM’s public IP address.
8. Open the IP address in your browser and check the IIS welcome page.
9. Take a screenshot of the successfully loaded IIS page.
10. Then delete the associated resource group.
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Create a VM with a suitable Windows image | Establish the basis for the web server | The Windows VM is successfully provisioned | ✅ |
| Open ports `3389` and `80` | Enable RDP and web access | The VM is accessible via RDP and HTTP | ✅ |
| Run `Install-WindowsFeature -name Web-Server -IncludeManagementTools` | Install IIS on the VM | The web server role is successfully installed | ✅ |
| Open the public IP in the browser | Check that the web server is working | The IIS welcome page loads | ✅ |
| Delete the resource group | Clean up resources and potential costs | All associated resources are removed | ✅ |

---

## Notes

- **IIS:** Internet Information Services is the web server integrated into Windows.
- **Public IP address:** This address makes the VM accessible from the internet.
- **Warning ⚠️:** Open management and web ports should only remain open for as long as they are needed for the exercise.

