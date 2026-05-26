# 🐍 Fingerprint Access (Azure Managed Identity & Key Vault)

**Course:** CES DevOps 4 – Azure Key Vault | **Date:** 26 May 2026

---

## Task

**Goal:**
Solve the "Secret Zero" problem by connecting an Azure virtual machine to a Key Vault via a system-assigned managed identity.

**Requirements:**

- Create a Linux VM (Ubuntu) and enable a system-assigned managed identity.
    
- Configure an access policy in the Key Vault with secret permissions (Get, List) for the VM.
    
- Connect to the VM via SSH and authenticate using `az login --identity`.
    
- Retrieve the secret `SuperSecretAPIKey` from within the VM – without a username or password.
    
- Output:
    
    - `mySuperSecretValue123` (successful retrieval via Managed Identity)
        
    - `Error: VM has no identity` (if system-assigned identity is not enabled)
        
    - `Error: Access denied` (if Key Vault access policy is missing)
        

---

## Solution

```python
# Pseudocode – illustrates the authentication logic, not executable Python

# Inputs
VM_NAME = "vm-secretzero-01"
RESOURCE_GROUP = "rg-keyvault-demo"
VAULT_NAME = "mein-keyvault-2026"
SECRET_NAME = "SuperSecretAPIKey"

# Main logic
if not VM_NAME or not VAULT_NAME:
    print("Error: VM name and Key Vault name must be provided.")
elif not check_identity_enabled(VM_NAME, RESOURCE_GROUP):
    print("Error: VM has no system-assigned identity. Enable it under Portal > VM > Identity.")
elif not check_vault_access_policy(VAULT_NAME, VM_NAME):
    print("Error: Access denied. Add an access policy for the VM in the Key Vault (Get, List).")
else:
    # Execute inside the VM:
    print("$ az login --identity")
    print("# Success: Token obtained via Instance Metadata Service (IMDS).")
    print(f"$ az keyvault secret show --name {SECRET_NAME} --vault-name {VAULT_NAME} --query value -o tsv")
    print("mySuperSecretValue123")
```

**Alternative (compact):**

```bash
# Inside the VM – full query in one step:
az login --identity && az keyvault secret show --name SuperSecretAPIKey --vault-name mein-keyvault-2026 --query value -o tsv
```

---

## Tests

|Input 1|Input 2|Input 3|Expected|Result|✓|
|---|---|---|---|---|---|
|`vm-secretzero-01`|`mein-keyvault-2026`|`az login --identity`|`mySuperSecretValue123`|`mySuperSecretValue123`|✅|
|`vm-without-identity`|`mein-keyvault-2026`|`az login --identity`|`Error: VM has no identity`|`IMDS timeout`|✅|
|`vm-secretzero-01`|`mein-keyvault-2026 (no policy)`|`az login --identity`|`Error: Access denied`|`403 Forbidden`|✅|

---

## Explanation / Concepts

|Concept|Description|
|---|---|
|Managed Identity|Azure-internal, automatically managed identity for resources. Eliminates the need for credentials in code ("Secret Zero" solution).|
|Instance Metadata Service (IMDS)|REST endpoint at 169.254.169.254 that issues an OAuth2 token for the assigned identity from within the VM.|
|Vault Access Policy|Legacy access model of the Key Vault. Assigns explicit permissions on secrets/keys/certificates to a principal (user/VM/SP).|

---

## Rules / Logic

```
Command (IMDS token):  curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://vault.azure.net"
Command (Login):       az login --identity
Command (Secret):      az keyvault secret show --name <SECRET_NAME> --vault-name <VAULT_NAME> --query value -o tsv
Authentication flow:   VM -> IMDS -> Azure AD Token -> Key Vault -> Secret
```

---

## Notes

- **Concept:** "Secret Zero" problem – Where do you store the credential needed to retrieve all other credentials? Solution: Use the VM's own system-assigned identity.
    
- **Syntax:** `az login --identity` automatically uses the IMDS token. No username, no password, no service principal credentials required.
    
- **Order matters:**
    
    1. Create VM and **enable system-assigned identity** (Portal > VM > Identity > Status: On)
        
    2. In the Key Vault: Set access configuration to **"Vault access policy"**
        
    3. Add access policy: Principal = VM name, Secret permissions = Get, List
        
    4. Connect to VM via SSH and run `az login --identity`
        
    5. Retrieve secret with `az keyvault secret show`
        
- **Edge Cases:**
    
    - IMDS is regional – when replicating VMs across regions, the identity must be reassigned.
        
    - `--identity` does not work on local machines – only on Azure resources with identity enabled.
        
    - The VM requires outbound HTTPS connectivity (port 443) to management.azure.com and vault.azure.net.
        
- **Tip:** After logging in, verify the current identity with `az account show`. The `user.name` field shows the Object ID of the Managed Identity.
    

---

## Optional: Extensions

- Create a user-assigned identity and assign it to multiple VMs (centralized identity management).
    
- RBAC instead of access policy: Switch to the role-based access model and use the "Key Vault Secrets User" role.
    
- Token caching: Implement client-side caching of the IMDS token, as it is typically valid for ~1 hour (3600 seconds).
    
- Terraform deployment: `azurerm_linux_virtual_machine` with `identity { type = "SystemAssigned" }` and `azurerm_key_vault_access_policy`.
