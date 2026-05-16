# Virtual Machines & Compute

## 📊 Summary based on the 80/20 principle

### 1. Virtual machines are the classic IaaS building block
A VM is a software-based computer in Azure. The 80/20 core principle is: understand the hypervisor, the VM components vCPU, vRAM, vDisk and vNIC, and the responsibilities you still bear yourself with IaaS.

### 2. Step-by-step core process
1. Select an operating system image and a suitable VM size.
2. Connect the VM to the network, storage and access rules.
3. Install and maintain the guest operating system and your applications.
4. Scale or replace the model with PaaS or containers if less operational effort is required.

### 3. Interactive mode / Tool usage
The Azure CLI is well suited for creating a VM in a reproducible manner rather than via a click in the portal. This allows you to see very clearly which parameters are actually necessary.

### 4. Key concepts with code examples
- **Hypervisor:** It divides physical host resources into isolated virtual machines.
- **vCPU, vRAM, vDisk, vNIC:** These are the core components of every Azure VM.
- **Guest OS:** With IaaS, you are responsible for patches, hardening and application maintenance yourself.
- **Compute models:** VM is just one model; App Service, containers and serverless services reduce administrative overhead.

```bash
az vm create \
  --resource-group rg-compute \
  --name vm-web-01 \
  --image Ubuntu2204 \
  --size Standard_B2s \
  --admin-username azureuser \
  --generate-ssh-keys
```

### 5. Comparison: VM vs. PaaS or Containers
- VMs give you the most control over the operating system and software stack.
- PaaS takes care of OS and platform maintenance for you, but with less granular control.
- Containers start up faster and are lighter, but share the host kernel.

### 6. Why is this important / Benefits
This topic forms the basis for almost every subsequent cloud architecture, because compute is where applications actually run and can be attacked.

**Quick-start checklist**
- ☐ I can explain what a hypervisor does.
- ☐ I am familiar with the building blocks vCPU, vRAM, vDisk and vNIC.
- ☐ I know what responsibilities I have in IaaS.
- ☐ I can broadly distinguish between VMs, containers and PaaS.

**Key point**
A VM is a rented computer in the cloud, but security and operations above the hypervisor remain your responsibility.

---

## Table 1: Tools used
| Tool | Description |
|---|---|
| Azure Virtual Machines | IaaS service for virtual servers |
| Azure CLI | Creates and manages VMs via commands |
| SSH | Secure remote access to Linux VMs |
| Azure Portal | Graphical interface for VM deployment and diagnostics |

## Table 2: Technical terms
| Term | Meaning |
|---|---|
| Hypervisor | Software that runs virtual machines on physical hardware |
| Guest OS | Operating system within the virtual machine |
| vNIC | Virtual network interface card of a VM |
| IaaS | Infrastructure as a Service with a high degree of self-management |

## Table 3: Key terms
| Term | Meaning |
|---|---|
| host | Host system |
| image | Template for a VM |
| instance | Running instance |
| patch | Security or functional update |


