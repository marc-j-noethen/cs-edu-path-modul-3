# 🐍 Vaulting Expectations (Key Vault Data Plane & RBAC)

**Course:** CES DevOps 4 – Azure Key Vault | **Date:** 26 May 2026

---

## Task

**Goal:**
Provision a Key Vault and programmatically retrieve a secret, overcoming access control barriers.

**Requirements:**

- Create a new Azure Key Vault and store a secret named `DatabaseString`.
    
- The plaintext value must NOT be viewable via the Azure portal – only via CLI.
    
- Identify and fix the permission barrier (management plane vs. data plane).
    
- Assign the "Key Vault Secrets User" role to your user account and successfully retrieve the secret.
    
- Output:
    
    - `Server=tcp:mydb.database.windows.net;Database=AppDB;User ID=admin;Password=***` (successful retrieval)
        
    - `Error: Access denied. Caller lacks permissions to perform action on 'secrets'` (without RBAC role)
        
    - `Error: User is not authorized to read secrets` (Azure RBAC missing on data plane)
        

---

## Solution

```python
# Pseudocode – illustrates the permission-check logic, not executable Python

# Inputs
VAULT_NAME = "mein-keyvault-2026"
RESOURCE_GROUP = "rg-keyvault-demo"
SECRET_NAME = "DatabaseString"
USER_OBJECT_ID = "a1b2c3d4-e5f6-7890-abcd-ef1234567890"  # Your Azure AD Object ID

# Main logic
if not has_rbac_role(USER_OBJECT_ID, "Key Vault Secrets User", VAULT_NAME):
    print("Error: Access denied. Assign the 'Key Vault Secrets User' role to the user.")
    print("Command: az role assignment create --role 'Key Vault Secrets User' --assignee-object-id {} --scope /subscriptions/{}/resourceGroups/{}/providers/Microsoft.KeyVault/vaults/{}".format(
        USER_OBJECT_ID, "<SUBSCRIPTION_ID>", RESOURCE_GROUP, VAULT_NAME
    ))
elif not SECRET_NAME:
    print("Error: Secret name must not be empty.")
else:
    # Azure CLI command
    print(f"$ az keyvault secret show --name {SECRET_NAME} --vault-name {VAULT_NAME} --query value -o tsv")
    print("Server=tcp:mydb.database.windows.net;Database=AppDB;User ID=admin;Password=***")
```

**Alternative (compact):**

```bash
# One-liner after RBAC assignment:
az keyvault secret show --name DatabaseString --vault-name mein-keyvault-2026 --query value -o tsv
```

---

## Tests

|Input 1|Input 2|Input 3|Expected|Result|✓|
|---|---|---|---|---|---|
|`mein-keyvault-2026`|`DatabaseString`|`Key Vault Secrets User assigned`|`Server=tcp:mydb...`|`Server=tcp:mydb...`|✅|
|`mein-keyvault-2026`|`DatabaseString`|`No RBAC role`|`Access denied`|`403 - Forbidden`|✅|
|`mein-keyvault-2026`|`NonExistent`|`Key Vault Secrets User assigned`|`SecretNotFound`|`Secret does not exist`|✅|

---

## Explanation / Concepts

|Concept|Description|
|---|---|
|Management Plane|Control plane for CRUD operations on Azure resources themselves (e.g. creating/deleting a Key Vault). Permissions via Azure RBAC or Owner/Contributor roles.|
|Data Plane|Access to the actual sensitive data within a resource (e.g. reading secrets). Requires separate permissions such as "Key Vault Secrets User".|
|Azure RBAC|Role-based access control. A more modern, fine-grained model compared to vault access policies. Supports conditional access and PIM.|

---

## Rules / Logic

```
Step 1:  Create Key Vault (management plane: Owner/Contributor)
Step 2:  Store secret (data plane: Key Vault Secrets Officer)
Step 3:  Assign RBAC role: az role assignment create --role "Key Vault Secrets User" --assignee <USER> --scope <VAULT_SCOPE>
Step 4:  Retrieve secret: az keyvault secret show --name <NAME> --vault-name <VAULT> --query value -o tsv
Important: Management plane permissions (Owner) do NOT automatically grant data plane rights.
```

---

## Notes

- **Concept:** Separation of management and data plane – A Key Vault owner is not automatically allowed to read all secrets. This is a fundamental security principle in Azure (Zero Trust).
    
- **Syntax:** `--assignee-object-id` uses the Object ID from Azure AD (not the email address) to avoid issues with guest users or duplicate names.
    
- **Order matters:**
    
    1. Create Key Vault in the Azure portal (Standard tier, soft-delete enabled)
        
    2. Create secret `DatabaseString` in the portal under "Secrets" (value: connection string)
        
    3. In the portal: Key Vault > Access control (IAM) > Add role assignment
        
    4. Select the "Key Vault Secrets User" role, add your own user account as a member
        
    5. Wait ~5–10 minutes (RBAC propagation is not immediate!)
        
    6. Run CLI command: `az keyvault secret show --name DatabaseString --vault-name <VAULT> --query value -o tsv`
        
- **Edge Cases:**
    
    - RBAC propagation delay: The role can take up to 10 minutes to take effect. Immediate retry results in a 403.
        
    - If the Key Vault is in access policy mode, RBAC will not work – switching to "Azure role-based access control" is required.
        
    - Guest users (external Azure AD accounts) require explicit RBAC assignments – inherited group memberships often propagate with a delay.
        
- **Tip:** Check your current permissions in advance with `az keyvault list` (management plane) vs. `az keyvault secret list --vault-name <VAULT>` (data plane). The former often works, the latter does not.
    

---

## Optional: Extensions

- Enable PIM (Privileged Identity Management): Make "Key Vault Secrets User" an eligible role with time-limited activation.
    
- Private Link integration: Access Key Vault via a private endpoint, disable public network access.
    
- Monitoring: Enable Diagnostic Settings to log all secret retrievals to Log Analytics (audit trail).
    
- Terraform: `azurerm_role_assignment` with `role_definition_name = "Key Vault Secrets User"` and `azurerm_key_vault_secret` resource.
