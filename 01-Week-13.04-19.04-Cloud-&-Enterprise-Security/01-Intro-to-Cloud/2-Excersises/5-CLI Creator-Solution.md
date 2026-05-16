# 🖥️ CLI Creator - Managing resource groups and virtual machines using the Azure CLI

**Course:** Cloud & Enterprise Security - Intro to Cloud | **Date:** 13 April 2026

---

## Task

**Objective:** Use the Azure Cloud Shell to create a resource group and a virtual machine, check the resources created, and then remove everything again via the CLI.

**Requirements:**
- Create the resource group `IntroAzureRG` in `germanywestcentral`.
- Create the virtual machine `my-VM` with Ubuntu 24.04, size `Standard_D2s_v3` and zone `1`.
- List the resource groups and resources, and delete the resource group including all resources it contains.

---

## Solution

### Creation and verification

```bash
az group create --name IntroAzureRG --location germanywestcentral
az vm create \
  --resource-group IntroAzureRG \
  --name my-VM \
  --image Canonical:ubuntu-24_04-lts:server:latest \
  --size Standard_D2s_v3 \
  --zone 1 \
  --admin-username azureuser \
  --generate-ssh-keys
az group list --query "[].{Name:name, Location:location}" -o table
az vm show --resource-group IntroAzureRG --name my-VM --query "{VM:name, ResourceGroup:resourceGroup, Location:location}" -o table
az resource list --resource-group IntroAzureRG -o table
```

### Clean-up

```bash
az group delete --name IntroAzureRG --yes
az group exists --name IntroAzureRG
```

### Output

```text
[... Output regarding the successful creation of the resource group and VM ...]

Name          ResourceGroup  Location
----------- -  -------------  -------------------
my-VM         IntroAzureRG   germanywestcentral
my-VMVMNic    IntroAzureRG   germanywestcentral
my-VM_OsDisk  IntroAzureRG   germanywestcentral
...

false
```

---

## Analysis

| Command/Step | Purpose | Expected result | ✓ |
|----------------|-------|---------------------|---|
| `az group create --name IntroAzureRG --location germanywestcentral` | Creates the target resource group | The resource group is successfully created | ✅ |
| `az vm create ...` | Creates the virtual machine with the required parameters | The VM and dependent resources are automatically provisioned | ✅ |
| `az group list ...` | Displays all resource groups in the subscription | `IntroAzureRG` appears in the overview | ✅ |
| `az resource list --resource-group IntroAzureRG -o table` | Lists all resources created in the group | VM, NIC, disk and other components are visible | ✅ |
| `az group delete --name IntroAzureRG --yes` | Removes the entire resource group and its contents | All associated resources are deleted | ✅ |
| `az group exists --name IntroAzureRG` | Checks the clean-up | Output `false` confirms successful deletion | ✅ |

---

## Notes

- **Automatic secondary resources:** When creating a VM, Azure often creates additional resources such as a network interface, public IP and managed disks.
- **Deleting the resource group:** Deleting the resource group is the quickest way to consistently remove all associated resources.
- **Warning ⚠️:** Creating a VM may incur costs; it is therefore essential to clean up after the test.


