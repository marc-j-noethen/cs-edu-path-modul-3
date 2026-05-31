# 🖥️ The Cold Shoulder - Installing Nginx on an Ubuntu VM and making it accessible

**Course:** Cloud & Enterprise Security - Virtual Machines & Compute | **Date:** 14 April 2026

---

## Task

**Objective:** Deploy an Ubuntu VM using the Azure CLI, install Nginx using the VM extension, and then make the web server accessible from the internet.

**Requirements:**
- Create an Ubuntu VM using the `Ubuntu2204` image.
- Complete the command `az vm extension set` with the missing line to run the Nginx installation script.
- Enable web access to the VM and, optionally, delete the resource after completing the follow-up exercise.

---

## Solution

### Azure CLI commands

```bash
resourcegroup="IntroAzureRG"
location="germanywestcentral"
vmname="my-vm"
username="azureuser"

az group create --name $resourcegroup --location $location

az vm create \
  --resource-group $resourcegroup \
  --name $vmname \
  --image Ubuntu2204 \
  --admin-username $username \
  --generate-ssh-keys

az vm extension set \
  --resource-group $resourcegroup \
  --vm-name $vmname \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --version 2.1 \
  --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"]}' \
  --protected-settings '{"commandToExecute":"./configure-nginx.sh"}'

az vm open-port --resource-group $resourcegroup --name $vmname --port 80
```

### Output

```text
{
  "provisioningState": "Succeeded",
  "type": "Microsoft.Compute/virtualMachines/extensions"
}

{
  "openPort": 80,
  "status": "Succeeded"
}
```

---

## Analysis

| Command/Step | Purpose | Expected result | ✓ |
|----------------|-------|---------------------|---|
| `az vm create ... --image Ubuntu2204 ...` | Creates the Ubuntu VM | The Linux VM is successfully provisioned | ✅ |
| Missing line `--protected-settings '{"commandToExecute":"./configure-nginx.sh"}'` | Runs the downloaded script on the VM | The Custom Script Extension successfully executes the Nginx setup | ✅ |
| `az vm extension set ...` | Automatically installs Nginx via the VM extension | The extension deployment completes with `Succeeded` | ✅ |
| `az vm open-port ... --port 80` | Allows HTTP access from the internet | The Nginx homepage is accessible in the browser | ✅ |
| Browser request to the public IP | Visual check of the web server | The Nginx default page is displayed | ✅ |

---

## Notes

- **Custom Script Extension:** This Azure extension downloads scripts and executes them after the VM has been deployed.
- **Command syntax matters:** `commandToExecute` must reference the downloaded filename exactly; `./ configure-nginx.sh` would fail because of the stray space.
- **Protected Settings:** `commandToExecute` should be passed as a protected parameter to ensure more secure and robust execution.
- **Warning ⚠️:** If port `80` is not opened, Nginx will run on the VM but will not be accessible from the outside.
