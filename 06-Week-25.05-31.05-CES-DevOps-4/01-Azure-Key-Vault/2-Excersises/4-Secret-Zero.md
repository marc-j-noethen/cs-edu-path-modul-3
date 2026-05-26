# 🐍 Secret Zero (VM Identity Authentication Without Credentials)

**Course:** CES DevOps 4 – Azure Key Vault | **Date:** 26 May 2026

---

## Task

**Goal:**
Solve the "Secret Zero" problem by authenticating an infrastructure resource directly with a Key Vault.

**Requirements:**

- Deploy an Azure virtual machine without any stored credentials.
    
- The VM must natively prove its own identity to the Key Vault – no tokens, no config files, no passwords on the VM.
    
- Link the VM to the Key Vault and retrieve the secret directly from the VM terminal.
    
- Document the structural identity linkage in the Azure portal.
    
- Output:
    
    - `mySuperSecretValue123` (successful retrieval via system-assigned identity)
        
    - `Error: No access token found. Ensure managed identity is enabled.` (identity not enabled)
        
    - `Error: The user, group or application does not have secrets get permission.` (missing access policy)
        

---

## Solution

```python
# Pseudocode – illustrates the identity-check logic, not executable Python

# Inputs
VM_NAME = "vm-secretzero-01"
RESOURCE_GROUP = "rg-keyvault-demo"
VAULT_NAME = "mein-keyvault-2026"
SECRET_NAME = "SuperSecretAPIKey"

# Main logic
if not is_system_identity_enabled(VM_NAME, RESOURCE_GROUP):
    print("Error: No access token found. Ensure managed identity is enabled.")
    print("Portal > VM > Identity > System assigned > Status: On > Save")
elif not has_vault_access_policy(VAULT_NAME, get_vm_principal_id(VM_NAME, RESOURCE_GROUP)):
    print("Error: The user, group or application does not have secrets get permission.")
    print(f"Add an access policy in Key Vault '{VAULT_NAME}':")
    print(f"  Principal: {VM_NAME}")
    print(f"  Secret permissions: Get, List")
else:
    # Execute on the VM (no credentials required):
    # Step 1: az login --identity
    # Expected output:
    # [
    #   {
    #     "environmentName": "AzureCloud",
    #     "homeTenantId": "...",
    #     "id": "...",
    #     "isDefault": true,
    #     "name": "MSI@<VM_NAME>",
    #     "state": "Enabled",
    #     "tenantId": "...",
    #     "user": {
    #       "name": "systemAssignedIdentity",
    #       "type": "servicePrincipal"
    #     }
    #   }
    # ]

    # Step 2: retrieve the secret
    print(f"$ az keyvault secret show --name {SECRET_NAME} --vault-name {VAULT_NAME} --query value -o tsv")
    print("mySuperSecretValue123")
```

**Alternative (compact):**

```bash
# Full workflow on the VM in one command:
az login --identity && az keyvault secret show --name SuperSecretAPIKey --vault-name mein-keyvault-2026 --query value -o tsv
```

---

## Tests

|Input 1|Input 2|Input 3|Expected|Result|✓|
|---|---|---|---|---|---|
|`vm-secretzero-01 (Identity: On)`|`mein-keyvault-2026 (Policy: Get/List)`|`az login --identity`|`mySuperSecretValue123`|`mySuperSecretValue123`|✅|
|`vm-secretzero-01 (Identity: Off)`|`mein-keyvault-2026`|`az login --identity`|`Error: No access token found`|`IMDS not reachable`|✅|
|`vm-secretzero-01 (Identity: On)`|`mein-keyvault-2026 (no policy)`|`az login --identity`|`Error: secrets get permission denied`|`HTTP 403 Forbidden`|✅|

---

## Explanation / Concepts

|Concept|Description|
|---|---|
|Secret Zero|The fundamental problem: Where do you store the first credential needed to retrieve all others? With Managed Identities, this problem is eliminated entirely.|
|System-Assigned Identity|An identity that is lifecycle-bound to a single Azure resource. When the VM is deleted, the identity is automatically removed as well.|
|OAuth2 Token Flow|VM requests an access token via IMDS (169.254.169.254) -> Azure AD validates -> Token is used for Key Vault API calls. Completely credential-free.|

---

## Rules / Logic

```
Step 1:  Create VM + enable system-assigned identity (Portal > VM > Identity)
Step 2:  Key Vault access policy: Principal = VM name, Secret Permissions = Get, List
Step 3:  Establish SSH connection to the VM
Step 4:  az login --identity (automatic token acquisition via IMDS)
Step 5:  az keyvault secret show --name <SECRET> --vault-name <VAULT> --query value -o tsv
Token endpoint:  http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://vault.azure.net
```

---

## Notes

- **Concept:** "Completely without passwords" – The VM itself is the key. The Instance Metadata Service (IMDS) automatically issues an OAuth2 bearer token that is validated against Azure AD and Key Vault.
    
- **Syntax:** `az login --identity` without additional parameters uses the system-assigned identity. For user-assigned identities: `az login --identity --username /subscriptions/.../userAssignedIdentities/<NAME>`.
    
- **Order matters:**
    
    1. Create VM in the Azure portal (Ubuntu 22.04 LTS recommended)
        
    2. In VM settings: **Identity > System assigned > On > Save** (wait until ARM deployment is complete)
        
    3. Navigate to the Key Vault: select **Access configuration > Vault access policy**
        
    4. **Access policies > + Add**: Secret permissions = Get, List; Principal = search and select VM name
        
    5. Connect to VM via SSH: `ssh azureuser@<VM_PUBLIC_IP>`
        
    6. Run `az login --identity` – should complete successfully without any credential input
        
    7. Retrieve secret with `az keyvault secret show`
        
- **Edge Cases:**
    
    - IMDS is only reachable from within the VM (169.254.169.254 is a link-local address). External calls are technically impossible.
        
    - If the VM has a firewall/NSG, outbound HTTPS traffic to *.vault.azure.net and management.azure.com must be allowed.
        
    - When using Azure RBAC instead of an access policy: The "Key Vault Secrets User" role must be assigned to the VM, not an access policy.
        
- **Tip:** The IMDS token endpoint returns a JSON with `access_token`. The Azure CLI wraps this flow elegantly. For custom code, the token can also be obtained directly via `curl` and sent in the `Authorization: Bearer <TOKEN>` header.
    

---

## Optional: Extensions

- User-assigned Managed Identity: Create an identity and assign it to multiple VMs (centralized secret management).
    
- Federated Identity (Workload Identity): For Kubernetes pods – the pod identity is linked to Azure AD via OIDC, no secret on the cluster.
    
- Token inspection: Decode the JWT token from IMDS (e.g. on jwt.ms) – contains `appid`, `oid`, `iss` and the audience `https://vault.azure.net`.
    
- Terraform: `azurerm_linux_virtual_machine` with `identity { type = "SystemAssigned" }` + `azurerm_key_vault_access_policy` with `object_id = azurerm_linux_virtual_machine.example.identity[0].principal_id`.
