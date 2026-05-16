# 🖥️ Shield Wall - Securing a subnet using an NSG and a restrictive SSH rule

**Course:** CES DevOps 3 - IaC & Terraform Intro | **Date:** 11 May 2026

---

## Task

**Objective:** Secure a subnet with a network security group and allow SSH access only from your own public IP address.

**Requirements:**
- Declare an NSG and an inbound rule for port 22.
- Restrict the rule to your own public `/32` address.
- Connect the NSG to the existing subnet via an association.
- Verify in the Azure portal that the subnet association is active.

---

## Solution

### Terraform code

```hcl
variable "admin_ip_cidr" {
  type = string
}

resource "azurerm_network_security_group" "ssh_nsg" {
  name                = "ssh-locked-nsg"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
}

resource "azurerm_network_security_rule" "allow_ssh_from_me" {
  name                        = "allow-ssh-from-my-ip"
  priority                    = 100
  direction                   = "Inbound"
  access                      = "Allow"
  protocol                    = "Tcp"
  source_port_range           = "*"
  destination_port_range      = "22"
  source_address_prefix       = var.admin_ip_cidr
  destination_address_prefix  = "*"
  resource_group_name         = azurerm_resource_group.rg.name
  network_security_group_name = azurerm_network_security_group.ssh_nsg.name
}

resource "azurerm_subnet_network_security_group_association" "subnet_assoc" {
  subnet_id                 = azurerm_subnet.training.id
  network_security_group_id = azurerm_network_security_group.ssh_nsg.id
}
```

### Execution

```bash
terraform apply -var='admin_ip_cidr=<your-public-ip>/32'
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| NSG + SSH rule | Define network access | Only TCP/22 from your own IP remains permitted. | ✅ |
| `/32` CIDR | Limit source as much as possible | The rule applies precisely to a single host rather than a broad network. | ✅ |
| Association resource | Bind NSG to subnet | The protection is actually linked to the subnet and not just created separately. | ✅ |

---

## Notes

- **NSG:** Network Security Groups are Azure’s central firewall rule objects at subnet and NIC level.
- **Own IP:** The task requires a restrictive rule, so `/32` is the appropriate scope.
- **Association:** In Azure, the association is technically a separate resource and not just an attribute of the subnet.

