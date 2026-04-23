# Project 4: Azure App Service with Azure SQL Database Integration

## 📌 Overview
This project demonstrates a cloud‑native application architecture using **Azure App Service** integrated with **Azure SQL Database**. The objective is to understand how Platform as a Service (PaaS) components communicate securely in Azure while complying with enterprise governance and security controls.

This project focuses on **architecture, security, and configuration design**, rather than application code complexity.

---

## 🏗️ Architecture
The web application is hosted on Azure App Service and connects to an Azure SQL Database hosted on a managed SQL Server. Networking, access, and configuration are controlled using Azure‑native security mechanisms.

Architecture flow:
User Browser → Azure App Service → Azure SQL Database

---

## 🔧 Azure Services Used
- Azure App Service (Windows)
- Azure SQL Server
- Azure SQL Database
- Azure SQL Firewall Rules
- Azure Portal Query Editor

---

## ⚙️ Implementation Steps (High Level)
1. Created an Azure SQL Server and Azure SQL Database
2. Configured SQL Server firewall rules
   - Allowed specific client IP for Query Editor access
   - Enabled access for Azure services
3. Accessed the database using Azure SQL Query Editor
4. Created tables and inserted sample data
5. Deployed a Web App using Azure App Service
6. Designed secure database connectivity via platform configuration
7. Validated PaaS‑to‑PaaS integration architecture

---

## 🔐 Security & Governance Considerations
- Azure SQL access restricted using firewall rules
- Database access limited to approved IPs and Azure services
- No public database exposure
- Application configuration governed by enterprise Azure policies
- App Service configuration UI restricted due to tenant governance controls
- Secret management and configuration handled at the platform level

---

## ✅ Outcome / Key Learnings
- Understood PaaS‑to‑PaaS application architecture in Azure
- Learned Azure SQL Database security and firewall management
- Gained experience with SQL Query Editor for database management
- Observed real‑world enterprise governance restrictions
- Learned that application configuration may be managed outside the Portal UI
- Strengthened architectural thinking beyond UI‑based implementation

---

## 📌 Notes
Due to enterprise governance policies in the Azure subscription, direct portal‑based editing of App Service application settings and connection strings was restricted. This reflects real‑world enterprise environments where configuration is managed via centralized governance, infrastructure‑as‑code, or DevOps pipelines.

The project emphasizes **secure design and compliance‑aware implementation**, which is aligned with production Azure environments.
