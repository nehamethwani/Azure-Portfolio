# Project: Automated Resource Cleanup

## 📌 Overview
This project demonstrates how to **identify and clean up non‑production Azure resources** using **Azure CLI** and **Azure Automation (design level)**.  
The objective is to control cloud costs and avoid resource sprawl by deleting resources based on **tags**, following enterprise governance practices.

Due to restricted RBAC permissions at the subscription level, resource cleanup was practically executed using Azure CLI, while Azure Automation was implemented and validated at the runbook level.

---

## 🏗️ Architecture
The cleanup process is designed to run automatically using Azure Automation and Managed Identity.  
In restricted enterprise environments, cleanup can also be executed manually using Azure CLI.

User / Schedule  
→ Azure Automation Runbook (PowerShell)  
→ Azure Resource Manager  
→ Tagged Azure Resources (Environment = Dev)

---

## 🔧 Azure Services Used
- Azure Resource Manager
- Azure Cloud Shell
- Azure CLI
- Azure Automation Account (Runbook)

---

## ⚙️ Implementation Steps
1. Identified cleanup criteria using resource tags (`Environment=Dev`).
2. Created and tagged non‑production Azure resources.
3. Validated target resources using Azure CLI.
4. Created an Azure Automation Account with system‑assigned managed identity.
5. Designed a PowerShell runbook for automated cleanup.
6. Tested the runbook logic in test mode.
7. Executed cleanup manually using Azure CLI due to RBAC restrictions.
8. Verified successful deletion of tagged resources.

---

## 📜 Azure Automation Runbook Script

The following PowerShell runbook is designed to delete all Azure resources tagged with `Environment=Dev`.  
The runbook authenticates securely using **Managed Identity**.

```powershell
Connect-AzAccount -Identity

$resources = Get-AzResource -Tag @{ Environment = "Dev" }

foreach ($resource in $resources) {
    Write-Output "Deleting resource: $($resource.Name)"
    Remove-AzResource -ResourceId $resource.ResourceId -Force
}
``
