# Virtual Networking

## 📊 Summary based on the 80/20 principle

### 1. VNets, subnets and NSGs control communication in Azure
The 80/20 principle is: a VNet provides you with private address space, subnets organise it, and NSGs determine which traffic is allowed or blocked. Added to this are public and private IPs, as well as Azure Bastion for secure admin access.

### 2. Step-by-step core process
1. Define the VNet’s private address space using CIDR.
2. Divide the VNet into subnets for front-end, back-end or management.
3. Connect resources to the appropriate subnet via NICs.
4. Secure traffic with NSG rules and reduce public admin access with Bastion.

### 3. Interactive mode / Tool usage
When working with Azure networks, it helps to first define the IP plan and then create the VNet and subnets in a reproducible manner via the CLI.

### 4. Key concepts with code examples
- **VNet:** Logically isolated private network area in Azure.
- **Subnet:** Segment within the VNet for separating roles and security zones.
- **NSG:** Stateful packet filter rules for inbound and outbound traffic.
- **Bastion:** Secure browser-based management without public SSH or RDP ports.

```bash
az network vnet create \
  --resource-group rg-net \
  --name vnet-main \
  --address-prefix 10.1.0.0/16 \
  --subnet-name snet-web \
  --subnet-prefix 10.1.1.0/24
```

### 5. Comparison: Private IP vs. Public IP
- Private IPs are used for internal communication within your VNet.
- Public IPs make resources accessible from the internet and increase the attack surface.
- Where possible, use bastion hosts, load balancers or reverse proxies instead of direct public admin access.

### 6. Why is this important / Benefits
Network errors are often security errors. Anyone who masters VNet, subnet and NSG already understands the bulk of cloud network hardening.

**Quick Start Checklist**
- ☐ I can explain the purpose of a VNet.
- ☐ I understand why subnets are important for segmentation.
- ☐ I know what an NSG rule does.
- ☐ I am aware of the security benefits of Azure Bastion.

**Key point**
In Azure, networking is not about cable management, but about deliberately defined isolation, accessibility and control via software.

---

## Table 1: Tools used
| Tool | Description |
|---|---|
| Azure Virtual Network | Private network space for Azure resources |
| Network Security Group | Filters permitted and blocked traffic |
| Azure Bastion | Secure browser access to VMs |
| Azure CLI | Automatically creates VNets, subnets and rules |

## Table 2: Technical terms
| Term | Meaning |
|---|---|
| CIDR | Notation for address spaces and subnet masks |
| VNet | Virtual network in Azure |
| NSG | Network security group with a set of rules |
| NIC | Network interface of a VM |

## Table 3: Key terms
| Term | Meaning |
|---|---|
| subnet | Subnet |
| inbound | Inbound |
| outbound | Outbound |
| isolation | Isolation |


