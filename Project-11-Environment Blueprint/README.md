# Project: Environment Blueprint

## 📌 Overview
This project demonstrates the implementation of a **standardized Environment Blueprint** in Azure using **ARM/Bicep** for infrastructure definition and **Azure Policy** for governance enforcement.  
The objective is to ensure that all environments (Dev, Test, Prod) are created in a **consistent, secure, and compliant manner**.

The project reflects real‑world enterprise practices where environment standards are enforced through Infrastructure as Code and Policy‑as‑Code.

---

## 🏗️ Architecture
The Environment Blueprint combines infrastructure deployment with governance controls.

ARM/Bicep Templates  
→ Baseline Infrastructure (Resource Group, Storage, Monitoring)  
→ Azure Policy Definitions  
→ Policy Initiative (Blueprint Enforcement)  
→ Governed Azure Environment  

All deployments are validated before execution to ensure compliance.

---

## 🔧 Azure Services & Technologies Used
- Azure Resource Manager (ARM)
- Azure Bicep
- Azure Policy
- Azure Policy Initiatives
- Azure CLI
- Azure Portal

---

## ⚙️ Implementation Steps
1. Defined environment standards and governance requirements.
2. Created modular Bicep templates for baseline infrastructure.
3. Parameterized templates for Dev, Test, and Prod environments.
4. Validated infrastructure templates using Azure CLI.
5. Created custom Azure Policy definitions for governance.
6. Grouped policies into a Policy Initiative.
7. Assigned policies at subscription scope (or validated assignment).
8. Tested enforcement by simulating non‑compliant deployments.

---

## ✅ Outcome
- Implemented a reusable and environment‑agnostic Azure blueprint.
- Standardized infrastructure provisioning using ARM/Bicep.
- Enforced governance through Azure Policy and initiatives.
- Ensured region and tagging compliance before resource creation.
- Demonstrated enterprise‑grade environment design and validation.

