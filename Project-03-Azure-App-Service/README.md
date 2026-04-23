# Project 3: Azure App Service Web Application (PaaS)

## 📌 Overview
This project demonstrates hosting a web application using **Azure App Service**, showcasing the **Platform as a Service (PaaS)** model in Azure. The goal is to understand how Azure App Service simplifies application hosting by abstracting infrastructure management, while providing built-in scalability, security, and availability.

---

## 🏗️ Architecture
The application is hosted on Azure App Service and accessed via an Azure‑managed public endpoint. All infrastructure management, patching, and scaling are handled by the platform.

Architecture flow:
User Browser → Azure App Service → Web Application Content

---

## 🔧 Azure Services Used
- Azure App Service (Windows / Linux)
- App Service Plan
- Kudu (Advanced Tools / SCM site)

---

## ⚙️ Implementation Steps (High Level)
1. Created an Azure App Service using a managed App Service Plan
2. Selected a runtime stack (.NET / Node.js)
3. Verified the default application endpoint
4. Deployed static web content using Kudu (Advanced Tools)
5. Configured startup behavior and default documents
6. Validated application accessibility using the Azure‑provided URL

---

## 🔐 Security & Governance Considerations
- No direct access to the underlying operating system
- Platform‑managed OS patching and runtime updates
- HTTPS enabled by default using Azure‑managed certificates
- Reduced attack surface compared to VM‑based hosting
- Configuration governed by enterprise Azure policies

---

## ✅ Outcome / Key Learnings
- Understood differences between IaaS (VM) and PaaS (App Service)
- Learned how App Service abstracts infrastructure management
- Gained experience with Kudu for application deployment
- Observed runtime‑specific behavior and startup configuration
- Experienced real‑world governance constraints in Azure App Service
- Identified scenarios where App Service is preferred over VMs

---

## 📌 Notes
This project intentionally focuses on **platform behavior and architecture** rather than application code complexity. It reflects real‑world enterprise usage of Azure App Service where infrastructure concerns are handled by the platform.
