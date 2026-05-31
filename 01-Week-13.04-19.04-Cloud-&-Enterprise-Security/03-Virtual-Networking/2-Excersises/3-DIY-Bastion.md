# 🖥️ DIY Bastion - Making a private web VM accessible via a jumpbox and SOCKS proxy

**Course:** Cloud & Enterprise Security – Virtual Networking | **Date:** 15 April 2026

---

## Task

**Objective:** Set up an Azure environment with a segmented VNet, a private web VM and a secure jumpbox, so that the internal VM’s website is only accessible locally via an SSH SOCKS proxy.

**Requirements:**
- Create a VNet with separate public and private subnets.
- Deploy a private VM without a public IP and allow web access only from the jumpbox subnet.
- Operate a jumpbox with a public IP, SSH key authentication and SSH access exclusively from its own public IP address.

---

## Solution

### Architecture and Deployment

```bash
resourcegroup="rg-diy-bastion"
location="germanywestcentral"
vnet="vnet-diy"
publicsubnet="snet-public"
privatesubnet="snet-private"
jumpnsg="nsg-jumpbox"
privnsg="nsg-private"
jumpvm="jumpbox-vm"
webvm="private-web-vm"
myip="<YOUR_PUBLIC_IP>/32"

az group create --name $resourcegroup --location $location

az network vnet create \
  --resource-group $resourcegroup \
  --name $vnet \
  --address-prefix 10.30.0.0/16 \
  --subnet-name $publicsubnet \
  --subnet-prefixes 10.30.1.0/24

az network vnet subnet create \
  --resource-group $resourcegroup \
  --vnet-name $vnet \
  --name $privatesubnet \
  --address-prefixes 10.30.2.0/24

az network nsg create --resource-group $resourcegroup --name $jumpnsg
az network nsg create --resource-group $resourcegroup --name $privnsg

az network nsg rule create \
  --resource-group $resourcegroup \
  --nsg-name $jumpnsg \
  --name allow-ssh-from-me \
  --priority 100 \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --source-address-prefixes $myip \
  --destination-port-ranges 22

az network nsg rule create \
  --resource-group $resourcegroup \
  --nsg-name $privnsg \
  --name allow-http-from-public-subnet \
  --priority 100 \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --source-address-prefixes 10.30.1.0/24 \
  --destination-port-ranges 80

az network vnet subnet update \
  --resource-group $resourcegroup \
  --vnet-name $vnet \
  --name $publicsubnet \
  --network-security-group $jumpnsg

az network vnet subnet update \
  --resource-group $resourcegroup \
  --vnet-name $vnet \
  --name $privatesubnet \
  --network-security-group $privnsg

az vm create \
  --resource-group $resourcegroup \
  --name $jumpvm \
  --image Ubuntu2204 \
  --vnet-name $vnet \
  --subnet $publicsubnet \
  --admin-username azureuser \
  --authentication-type ssh \
  --generate-ssh-keys

az vm create \
  --resource-group $resourcegroup \
  --name $webvm \
  --image Ubuntu2204 \
  --vnet-name $vnet \
  --subnet $privatesubnet \
  --public-ip-address \"\" \
  --admin-username azureuser \
  --authentication-type ssh \
  --generate-ssh-keys

az vm run-command invoke \
  --resource-group $resourcegroup \
  --name $webvm \
  --command-id RunShellScript \
  --scripts "sudo apt-get update && sudo apt-get install -y nginx"
```

### Access test with SOCKS proxy

```bash
JUMPBOX_IP="$(az vm list-ip-addresses \
  --resource-group $resourcegroup \
  --name $jumpvm \
  --query \"[].virtualMachine.network.publicIpAddresses[*].ipAddress\" \
  -o tsv)"

ssh -D 8080 -C -N azureuser@$JUMPBOX_IP
```

### Implementation

**Step 1:** The jumpbox is deployed in the public subnet with a public IP address, whilst the private web VM is deployed in the private subnet without a public IP address.

**Step 2:** The NSGs ensure that SSH access to the jumpbox is only permitted from its own IP address and that the private web VM only accepts web traffic from the jumpbox subnet.

**Step 3:** Locally, a SOCKS proxy is set up using `ssh -D 8080 -C -N azureuser@$JUMPBOX_IP`.

**Step 4:** In the browser, a SOCKS5 proxy is configured to `127.0.0.1:8080` and then the private address of the internal VM, for example `http://10.30.2.4`, is opened.

---

## Analysis

| Component | Purpose | Expected result | ✓ |
|----------|-------|---------------------|---|
| Public and private subnet | Establish network segmentation | Jumpbox and target VM are clearly separated | ✅ |
| Jumpbox without password authentication | Implement secure admin access | Login is only via SSH key | ✅ |
| NSG rule `allow-ssh-from-me` | Restrict SSH access to the jumpbox to the local IP | Only the local client can use port 22 | ✅ |
| Private VM without public IP | Prevent direct internet access | The target VM is not publicly accessible | ✅ |
| SOCKS proxy via `ssh -D 8080 ...` | Tunnel browser access through the jump host | The private VM’s website is visible locally via the private IP | ✅ |

---

## Notes

- **Jumpbox:** A jumpbox is a deliberately exposed administration host through which internal systems are accessed indirectly.
- **SOCKS proxy:** Using `ssh -D` allows local browser traffic to be securely tunnelled through a remote host.
- **Least exposure:** For this exercise, the private VM only needs inbound HTTP from the jumpbox subnet. An extra SSH rule on the private VM would unnecessarily widen the attack surface.
- **Warning ⚠️:** The private VM must not be assigned a public IP address, otherwise the core requirement of the exercise would be violated.
