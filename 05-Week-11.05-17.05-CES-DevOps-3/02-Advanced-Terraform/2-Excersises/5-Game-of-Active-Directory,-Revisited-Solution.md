# 🖥️ Game of Active Directory, Revisited – Deploying GOAD-Light with Wazuh in Azure

**Course:** CES DevOps 3 – Advanced Terraform | **Date:** 12 May 2026

---

## Task

**Objective:** Successfully deploy the GOAD-Light environment using Azure as the provider and the Wazuh extension based on the specified commit status, and validate it in the portal.

**Requirements:**
- Check out the GOAD repository to the exact commit specified.
- Install or deploy GOAD-Light with the `azure` provider.
- After successful lab installation, add the `wazuh` extension.
- Finally, check the resource status in Azure and the Wazuh dashboard; then destroy the environment.

---

## Solution

### GOAD Procedure

```bash
git clone https://github.com/Orange-Cyberdefense/GOAD.git
cd GOAD
git checkout 9606647a47a0792ef9240edc568d9858e415a8f3
./goad.sh
# in interactive mode:
set_lab GOAD-Light
set_provider azure
install
load <instance_id>
install_extension wazuh
```

### Final check

```text
1. In the Azure portal, verify that the GOAD-Light VMs and the additional Wazuh host are running.
2. Open the Wazuh dashboard in your browser.
3. Take screenshots of the dashboard and the Azure resource group containing all the created resources.
4. Once complete, delete the lab environment.
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| Check out the fixed commit | Establish a reproducible initial state | The deployment is based exactly on the state specified in the task. | ✅ |
| `set_lab GOAD-Light` + `set_provider azure` | Select correct lab variant and target platform | GOAD-Light is explicitly configured for Azure. | ✅ |
| `install_extension wazuh` | Add EDR/monitoring extension | The lab setup additionally includes the Wazuh server and agents on the target systems. | ✅ |
| Portal and dashboard check | Verify operational status | Both Azure resources and the Wazuh frontend are accessible and in use. | ✅ |

---

## Notes

- **GOAD:** Game of Active Directory is a deliberately vulnerable training environment for AD and detection scenarios.
- **Wazuh extension:** According to official GOAD documentation, this adds a Wazuh server and agents on the Windows machines.
- **Important:** The environment is large and potentially expensive; cleaning up after the exercise is mandatory.

