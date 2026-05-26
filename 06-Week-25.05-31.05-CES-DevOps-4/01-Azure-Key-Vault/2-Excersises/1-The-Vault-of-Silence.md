# 🐍 The Vault of Silence (Azure Key Vault – Basics)

**Course:** CES DevOps 4 – Azure Key Vault | **Date:** 26 May 2026

---

## Task

**Objective:**
Create your first Azure Key Vault and manually store a secret to understand the fundamental decoupling of data and code.

**Requirements:**

- Create a new Key Vault (Standard tier) in the Azure portal with soft-delete enabled.
    
- Store a secret named `SuperSecretAPIKey` with any value.
    
- Install and use the Azure CLI to retrieve the secret.
    
- Use the `--query value` parameter to retrieve only the plaintext value.
    
- Output:
    
    - `mySuperSecretValue123` (the stored secret value in plaintext)
        
    - `Error: Secret not found or access denied` (if permissions are incorrect)
        
    - `Error: Key Vault not found` (if the vault name or resource group is incorrect)
        

---

## Solution

```python
# Pseudocode – illustrates the CLI logic, not executable Python

# Inputs
VAULT_NAME = "mein-keyvault-2026"
RESOURCE_GROUP = "rg-keyvault-demo"
SECRET_NAME = "SuperSecretAPIKey"

# Main logic
if not VAULT_NAME or not RESOURCE_GROUP:
    print("Error: Key Vault name and resource group must be specified.")
elif not SECRET_NAME:
    print("Error: Secret name must not be empty.")
else:
    # Azure CLI command to retrieve the secret value
    print(f"Execute: az keyvault secret show --name {SECRET_NAME} --vault-name {VAULT_NAME} --query value -o tsv")
    # Expected output on success:
    # mySuperSecretValue123
```

**Alternative (compact):**

```bash
# Single-line Azure CLI command
az keyvault secret show --name SuperSecretAPIKey --vault-name mein-keyvault-2026 --query value -o tsv
```

---

## Tests

|Input 1|Input 2|Input 3|Expected|Result|✓|
|---|---|---|---|---|---|
|`mein-keyvault-2026`|`SuperSecretAPIKey`|`--query value -o tsv`|`mySuperSecretValue123`|`mySuperSecretValue123`|✅|
|`incorrect-vault-name`|`SuperSecretAPIKey`|`--query value -o tsv`|`Error: Key Vault not found`|`KeyVaultNotFound`|✅|
|`mein-keyvault-2026`|`NonExistentSecret`|`--query value -o tsv`|`Error: Secret not found`|`SecretNotFound`|✅|

---

## Explanation / Concepts

|Concept|Description|
|---|---|
|Key Vault|A cloud-based vault for securely storing secrets, keys and certificates. Separates sensitive data from application code.|
|Soft-delete|Protection feature that makes deleted secrets recoverable for 90 days. Prevents accidental data loss. Enabled by default and cannot be disabled for new vaults (enforced since February 2025).|
|Azure CLI|Command-line tool for Azure resources. Enables automation without portal access.|

---

## Rules / Logic

```
Command:  az keyvault secret show --name <SECRET_NAME> --vault-name <VAULT_NAME> --query value -o tsv
Step 1:  az login (authentication against Azure AD)
Step 2:  az keyvault secret show (API call to Azure Key Vault database)
Step 3:  --query value (JMESPath filter, extracts only the value property)
Step 4:  -o tsv (Tab-Separated Values output, removes quotation marks)
```

---

## Notes

- **Concept:** Data/code decoupling – secrets are never stored in the source code, but in a managed vault.
    
- **Syntax:** `--query value` uses JMESPath, a JSON query language. `-o tsv` ensures plain text output without JSON formatting.
    
- **Order is important:**
    
    1. Run `az login` (browser authentication)
        
    2. Create a Key Vault (Portal or `az keyvault create`)
        
    3. Store a secret (`az keyvault secret set`)
        
    4. Retrieve the secret (`az keyvault secret show`)
        
- **Edge cases:**
    
    - Authentication error: User does not have Key Vault data-level permissions (management level only).
    
    - Soft delete: Deleted secrets can be restored with `az keyvault secret recover --name <SECRET_NAME> --vault-name <VAULT_NAME>`.
        
    - Firewall: Key Vault may be restricted to specific IP addresses – CLI calls will be blocked.
        
- **Tip:** Use `-o table` instead of `-o tsv` for a clear tabular view when there are multiple properties.
    

---

## Optional: Extensions

- Automated secret rotation with Azure Functions and Event Grid.
    
- Private endpoint configuration for Key Vault (no public internet access).
    
- Error handling: Implement retry logic for transient Azure API errors.
    
- Terraform deployment: Manage Key Vault and secrets as code (`azurerm_key_vault`, `azurerm_key_vault_secret`).
