# 🖥️ Broken Connectivity – Diagnosing and resolving network issues using the Azure CLI

**Course:** Cloud & Enterprise Security – Virtual Networking | **Date:** 15 April 2026

---

## Task

**Objective:** Use the Azure CLI to determine the public IP of a VM, check the current NSG rules, add a missing HTTP rule, and restore access to the Nginx web server.

**Requirements:**
- Use the Linux VM `my-vm` created in the AZ-900 lab, the automatically generated resource group, and the NSG `my-vmNSG`.
- Add the missing Azure CLI commands to determine the IP address, view the NSG rules, and create an HTTP rule.
- Submit a screenshot of the successful output of the final `curl` command.

---

## Solution

### Azure CLI commands

```bash
IPADDRESS="$(az vm list-ip-addresses \
  --resource-group <YOUR_RESOURCE_GROUP_NAME> \
  --name my-vm \
  --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
  --output tsv)"

curl --connect-timeout 5 http://$IPADDRESS

az network nsg rule list \
  --resource-group <YOUR_RESOURCE_GROUP_NAME> \
  --nsg-name my-vmNSG \
  --query '[].{Name:name, Priority:priority, Port:destinationPortRange, Access:access}' \
  --output table

az network nsg rule create \
  --resource-group <YOUR_RESOURCE_GROUP_NAME> \
  --nsg-name my-vmNSG \
  --name allow-http \
  --protocol tcp \
  --priority 100 \
  --destination-port-range 80 \
  --access Allow

az network nsg rule list \
  --resource-group <YOUR_RESOURCE_GROUP_NAME> \
  --nsg-name my-vmNSG \
  --query '[].{Name:name, Priority:priority, Port:destinationPortRange, Access:access}' \
  --output table

curl --connect-timeout 5 http://$IPADDRESS
```

### Output

```text
Name              Priority  Port  Access
----------------  --------  ----  ------
allow-http        100       80    Allow
default-allow-ssh 1000      22    Allow

<h2>Welcome to Azure! My name is my-vm.</h2>
```

---

## Analysis

| Command | Purpose | Expected result | ✓ |
|--------|-------|---------------------|---|
| `az vm list-ip-addresses ...` | Reads the VM’s public IP | `IPADDRESS` contains the reachable public IP | ✅ |
| First `curl --connect-timeout 5 http://$IPADDRESS` | Checks web accessibility before the fix | The connection fails due to a missing port 80 rule | ✅ |
| `az network nsg rule list ...` | Displays the existing NSG rules | Only SSH or a similar minimal rule is visible | ✅ |
| `az network nsg rule create ... --destination-port-range 80 ...` | Adds the missing HTTP permission | A new `allow-http` rule is created | ✅ |
| Second `az network nsg rule list ...` | Verifies the configuration | The new Port 80 rule appears in the list | ✅ |
| Second `curl --connect-timeout 5 http://$IPADDRESS` | Checks the resolved state | The Nginx homepage loads successfully | ✅ |

---

## Notes

- **NSG:** A Network Security Group controls which network traffic is allowed or blocked to a VM or subnet.
- **Cause of error:** The web server was already running but was not accessible from the internet due to a missing incoming HTTP rule.
- **Warning ⚠️:** In real-world environments, only the absolutely necessary ports should be opened and, where possible, restricted to trusted source ranges.

