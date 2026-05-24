# Project: Auto-Scaling App Service

## 📌 Overview
This project demonstrates how to implement **scaling for Azure App Service** to handle varying workloads using **Application Insights and Azure Monitor**.  
The objective is to improve application performance during high traffic and optimize cost during low usage.

The project includes both:
- Manual scaling (implemented due to RBAC restrictions)
- Autoscaling strategy (designed using Azure Monitor)

---

## 🏗️ Architecture
The application is deployed on Azure App Service and monitored using Application Insights. Scaling decisions are based on application load and performance metrics.

User Traffic  
→ Azure App Service  
→ Application Insights (Metrics collection)  
→ Azure Monitor (Scaling evaluation)  
→ App Service Plan (Scale up/down)

---

## 🔧 Azure Services Used
- Azure App Service  
- Azure App Service Plan  
- Application Insights  
- Azure Monitor  

---

## ⚙️ Implementation Steps
1. Created an Azure App Service and App Service Plan.
2. Enabled Application Insights for monitoring.
3. Deployed a simple web application.
