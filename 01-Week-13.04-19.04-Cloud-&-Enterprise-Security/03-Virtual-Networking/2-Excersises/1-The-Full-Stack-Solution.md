# 🖥️ The complete setup – Setting up a VNet, a bastion server and two VMs in the portal

**Course:** Cloud and Enterprise Security – Virtual Networks | **Date:** 15 April 2026

---

## Task

**Objective:** Using the official Azure Quickstart guide, create a complete network environment comprising a virtual network, Azure Bastion and two virtual machines, and test mutual reachability using `ping`.

**Prerequisites:**
- Follow the official Quickstart guide `Quickstart: Create an Azure virtual network`.
- Deploy a VNet with Azure Bastion and two VMs in the same network.
- Submit a screenshot of a successful `ping` within a Bastion session between the two machines.

---

## Solution

### Steps in the portal

```text
1. Open the Quickstart guide for a virtual network in the Azure portal and follow the steps in the portal.
2. Create a new virtual network with a resource subnet and an AzureBastionSubnet.
3. Enable an Azure Bastion host so that the VMs are accessible without public management ports.
4. Deploy two Linux VMs in the same VNet, for example vm-1 and vm-2.
5. Do not open any public inbound ports for either of the two VMs.
6. First, connect to vm-1 via “Connect” > “Bastion”.
7. In the Bastion session on vm-1, run the command `ping -c 4 vm-2`.
8. Optionally, perform the reverse test from vm-2 to vm-1 using `ping -c 4 vm-1`.
9. Take a screenshot of the successful ping output in the Bastion session.
```

### Output

```text
azureuser@vm-1:~$ ping -c 4 vm-2
PING vm-2.internal.cloudapp.net (10.0.0.5) 56(84) bytes of data.
64 bytes from vm-2.internal.cloudapp.net (10.0.0.5): icmp_seq=1 ttl=64 time=1.83 ms
64 bytes from vm-2.internal.cloudapp.net (10.0.0.5): icmp_seq=2 ttl=64 time=0.98 ms
64 bytes from vm-2.internal.cloudapp.net (10.0.0.5): icmp_seq=3 ttl=64 time=0.86 ms
64 bytes from vm-2.internal.cloudapp.net (10.0.0.5): icmp_seq=4 ttl=64 time=0.89 ms
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Create VNet with bastion | Provide a secure management environment | The network contains a bastion subnet and a resource subnet | ✅ |
| Create two VMs in the same VNet | Enable internal communication | Both VMs are assigned private IP addresses in the same VNet | ✅ |
| Establish a connection via Bastion | Access without public SSH or RDP ports | The VM console opens securely in the browser | ✅ |
| Run `ping -c 4 vm-2` | Check name resolution and Layer 3 connectivity | Responses from the second VM are received | ✅ |
| Take a screenshot | Save proof for submission | Successful ping output is visible | ✅ |

---

## Notes

- **Azure Bastion:** Bastion enables secure access to VMs directly within the portal without exposing public management ports.
- **Virtual Network:** A VNet is the central networking layer for private communication between Azure resources.
- **Internal name resolution:** VMs in the same VNet can often reach each other directly via their hostname.

