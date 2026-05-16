# IAM Basics

## 📊 Summary based on the 80/20 principle

### 1. IAM answers two questions: Who are you and what are you allowed to do?
The core of the 80/20 principle is the separation of authentication and authorisation. In Azure, Microsoft Entra ID manages identities, RBAC assigns permissions, and the least privilege principle limits unnecessary access.

### 2. Step-by-step core process
1. Authenticate an identity, for example using a password and MFA.
2. Verify the identity in Microsoft Entra ID.
3. Assign a suitable role within the correct scope via RBAC.
4. Consistently minimise permissions in accordance with the principle of least privilege.

### 3. Interactive mode / Tool usage
In practice, you usually grant permissions at subscription, resource group or resource level. It is precisely this scope that determines how far a permission actually extends.

### 4. Key concepts with code examples
- **AuthN:** Verifies the identity of a person or application.
- **AuthZ:** Determines which actions are permitted after successful login.
- **Microsoft Entra ID:** Central identity and directory service in Azure.
- **RBAC:** Role assignment with scope and minimal permissions instead of blanket admin access.

```bash
az role assignment create \
  --assignee user@contoso.com \
  --role Reader \
  --scope /subscriptions/<subscription-id>/resourceGroups/rg-demo
```

### 5. Comparison: Authentication vs. Authorisation
- Authentication answers: Who are you?
- Authorisation answers: What are you allowed to do?
- Without successful authentication, proper authorisation cannot take place.

### 6. Why is this important / Benefits
IAM is the security foundation of the cloud. Incorrect roles or a lack of MFA often lead to a breach more quickly than technical vulnerabilities in a VM.

**Quick-start checklist**
- ☐ I can clearly distinguish between AuthN and AuthZ.
- ☐ I know what Microsoft Entra ID is responsible for.
- ☐ I understand RBAC, roles and scope.
- ☐ I am familiar with the principle of least privilege.

**Key point**
Good cloud security does not start with firewalls, but with well-managed identities and minimal permissions.

---

## Table 1: Tools used
| Tool | Purpose |
|---|---|
| Microsoft Entra ID | Manages users, groups and logins |
| RBAC | Controls permissions via roles |
| MFA | Increases security during sign-in |
| Azure CLI | Assigns roles reproducibly via command |

## Table 2: Technical terms
| Term | Meaning |
|---|---|
| Authentication | Verification that an identity is genuine |
| Authorisation | Verification of which actions are permitted |
| Scope | The level at which a role applies |
| Least Privilege | Granting only the minimum necessary rights |

## Table 3: Key terms
| Term | Meaning |
|---|---|
| identity | Identity |
| permission | Permission |
| assign | Assign |
| deny | Deny |


