# 🖥️ Tour de Portal – Complete the Microsoft Learn lab using the Portal and Cloud Shell

**Course:** Cloud & Enterprise Security – Intro to Cloud | **Date:** 13 April 2026

---

## Task

**Objective:** Complete the Microsoft Learn lab on navigating the Azure portal and display the current date and Azure CLI version in Cloud Shell.

**Requirements:**
- Complete the Microsoft Learn lab `exercise-explore-learn-sandbox` in your own subscription.
- Use Azure Cloud Shell for the command-line steps.
- Submit a screenshot showing the output for the current date and Azure CLI version.

---

## Solution

### Cloud Shell commands

```bash
date
az version --query '"azure-cli"' -o tsv
```

### Output

```text
Mon Apr 28 10:42:31 UTC 2026
2.xx.x
```

---

## Analysis

| Command | Purpose | Expected output | ✓ |
|--------|-------|----------------- --|---|
| `date` | Displays the current Cloud Shell system date | Date and time of the shell session | ✅ |
| `az version --query '"azure-cli"' -o tsv` | Returns only the installed Azure CLI version | A single version number such as `2.xx.x` | ✅ |
| Using Cloud Shell in the lab | Runs CLI steps directly in Azure | Commands run without local installation | ✅ |

---

## Notes

- **Azure Cloud Shell:** A browser-based shell with the Azure CLI pre-installed and optional Bash or PowerShell mode.
- **Microsoft Learn Sandbox:** The Sandbox provides a guided practice environment for lab tasks.
- **Check CLI version:** Checking the version is useful for verifying command behaviour and documentation against the installed version.

