# 🖥️ Creation pt.2 - Deploying a Windows VM and IIS using the Azure CLI

**Course:** Cloud & Enterprise Security - Virtual Machines & Compute | **Date:** 14 April 2026

---

## Task

**Objective:** Create a Windows VM using the Azure CLI, install IIS via a remote command, enable web access, and document the output of the installation command.

**Requirements:**
- Follow the Microsoft Quickstart guide to create a Windows VM using the Azure CLI.
- Install the IIS web server using `az vm run-command invoke` and open port `80`.
- Submit a screenshot of the output of the command used to install the web server and subsequently delete the resource group.

---

## Solution

### Azure CLI commands

```bash
resourcegroup="myResourceGroupCLI"
location="germanywestcentral"
vmname="myVM"
username="azureuser"

az group create --name $resourcegroup --location $location

# The Azure CLI prompts for the administrator password interactively
# if --admin-password is omitted.
az vm create \
  --resource-group $resourcegroup \
  --name $vmname \
  --image Win2022AzureEditionCore \
  --public-ip-sku Standard \
  --admin-username $username

az vm run-command invoke -g $resourcegroup \
  -n $vmname \
  --command-id RunPowerShellScript \
  --scripts "Install-WindowsFeature -name Web-Server -IncludeManagementTools"

az vm open-port --port 80 --resource-group $resourcegroup --name $vmname

az group delete --name $resourcegroup --yes
```

### Output

```text
{
  "value": [
    {
      "code": "ComponentStatus/StdOut/succeeded",
      "message": "Success Restart Needed Exit Code      Feature Result\n------- -------------- ---------      --------------\nTrue    No             Success        {Common HTTP Features, Default Document, Directory Browsing, HTTP Errors...}"
    }
  ]
}
```

---

## Analysis

| Command | Purpose | Expected result | ✓ |
|--------|-------|---------------------|---|
| `az group create --name $resourcegroup --location $location` | Creates the resource group | The resource group is successfully created | ✅ |
| `az vm create ... --image Win2022AzureEditionCore ...` | Creates the Windows VM via the CLI | The VM is running and has a public IP | ✅ |
| `az vm run-command invoke ...` | Installs IIS within the VM | The output confirms the successful installation of the Web Server role | ✅ |
| `az vm open-port --port 80 ...` | Opens HTTP access from the internet | The IIS page can be accessed later in the browser | ✅ |
| `az group delete --name $resourcegroup --yes` | Cleans up all lab resources | The resource group and its contents are deleted | ✅ |

---

## Notes

- **Run Command:** `az vm run-command invoke` allows PowerShell scripts to be executed remotely in a Windows VM.
- **Admin password:** For Windows VMs, `az vm create` prompts for a compliant password unless you explicitly pass `--admin-password`.
- **Region/SKU:** Depending on your subscription, you may need to adjust the region, size or image to match available SKUs.
- **Warning ⚠️:** By default, the CLI does not automatically open port `80` on a Windows VM; this must be explicitly opened for IIS access.
