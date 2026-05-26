# Azure Key Vault
#### 1. Key Concept

Azure Key Vault is a centralised, highly secure service for managing secrets, cryptographic keys and certificates. It enables the separation of secrets and code to avoid security risks such as ‘secret sprawl’ and ‘leaky code’.

---

#### 2. Step-by-step guide or core process

1. Identity assignment:
    - A managed identity (digital identity) is assigned to the application.
2. Request to Key Vault:
    - The application sends a request to the Key Vault URL (e.g. `https://my-vault.vault.azure.net`).
3. Authentication:
    - Key Vault verifies the identity and checks the access policy (access rights).
4. Authorisation:
    - Key Vault confirms whether the application is authorised to retrieve a specific secret.
5. Secure data access:
    - The secret is transmitted via an encrypted TLS connection and remains only in the application’s volatile memory.

---

#### 3. Interactive mode / REPL / Tool usage

- Azure Portal:
    - Create a Key Vault via the portal and configure Managed Identity for resources (e.g. VMs, App Services).
- Azure CLI:
    - Commands such as `az keyvault create` or `az keyvault secret set` enable the management of secrets.
- Integration into applications:
    - Use the Azure SDKs (e.g. `Azure.Security.KeyVault.Secrets`) to dynamically retrieve secrets.

---

#### 4. Code examples

Example: Retrieve a secret using the Azure SDK (C#)

csharp

```
var client = new SecretClient(new Uri("https://my-vault.vault.azure.net"), new DefaultAzureCredential());
var secret = client.GetSecret("my-secret-name");
Console.WriteLine($"Secret value: {secret.Value.Value}");
```

```
var client = new SecretClient(new Uri("https://my-vault.vault.azure.net"), new DefaultAzureCredential());
var secret = client.GetSecret("my-secret-name");
Console.WriteLine($"Secret value: {secret.Value.Value}");
```

---

#### 5. Platform comparison

|Insecure (Local configuration)|Secure (Key Vault)|
|---|---|
|Storage|Hardcoded or in `.env` files|
|Rotation|Code redeployment required|
|Visibility|Developers can see production secrets|
|History|No logs|

---

#### 6. Why is this important

- No secrets in the code:
    - No passwords in source code, configuration files or GitHub.
- Hardware-protected security:
    - Even if code is stolen, access will not work as the identity is bound to the VM.
- Dynamic resources:
    - Secrets are only retrieved when needed (‘on-demand’).

---

### ✅ Checklist: Key steps

- [ ]  Do not store secrets in source code or `.env` files.
- [ ]  Enable Managed Identity for Azure resources.
- [ ]  Configure access rights (Access Policy) precisely in Key Vault.
- [ ]  Retrieve secrets via Azure SDKs or CLI.
- [ ]  Check audit logs regularly to detect security incidents.

---

### 🛠 Tools

|Tool|Description|
|---|---|
|Azure Key Vault|Centralised service for managing secrets, keys and certificates.|
|Azure CLI|Command-line interface for creating and managing Key Vaults.|
|Azure Portal|Graphical interface for configuring Key Vaults and Managed Identity.|

---

### 🔑 Technical Terms

- Secrets: Opaque strings (e.g. database passwords, API tokens).
- Keys: Cryptographic keys for encryption/decryption.
- Certificates: Automated management of SSL/TLS certificates.
- Managed Identity: Azure-extended identity that does not require passwords.
- Access Policy: Rules for controlling who is allowed to access which secrets.
- TLS: Encryption layer for secure communication with Key Vault.

---

### 📌 Vocabulary

- Secret sprawl: The proliferation of secrets in code, configurations and Git repositories.
- Leaky code: Secrets that are visible in source code or commit histories.
- Secret Zero: The ‘secret of secrets’ (e.g. the password for Key Vault).
- Zero Credentials: No passwords in code or configurations.
- Dynamic Resource: Secrets are retrieved dynamically, not stored statically.

---

### 📌 Mnemonic

“Azure Key Vault protects secrets through centralisation, identity-based security and dynamic access – no passwords in code, no leaks, no history.”


