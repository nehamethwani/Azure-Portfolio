# Project: Multi-Region Deployment using Azure App Service & Azure Traffic Manager

## 📌 Overview
This project demonstrates a **multi-region web application deployment** in Azure using **Azure App Service** and **Azure Traffic Manager**. The objective is to improve application availability and resiliency by deploying the same application in multiple regions and routing user traffic through a global DNS-based traffic management solution.

---

## 🏗️ Architecture
The application is deployed in two Azure regions and accessed through Azure Traffic Manager, which routes traffic based on health and performance.

User  
→ Azure Traffic Manager  
→ Azure App Service (Region 1)  
→ Azure App Service (Region 2)

Traffic Manager monitors the health of each endpoint and directs users to the best available region.

---

## 🔧 Azure Services Used
- Azure App Service  
- Azure App Service Plan  
- Azure Traffic Manager  

---

## ⚙️ Implementation Steps
1. Created a resource group for the multi-region deployment.
2. Created App Service Plans in two different Azure regions.
3. Deployed identical Web Apps in each region.
4. Uploaded static HTML content to both Web Apps.
5. Verified that each App Service functions independently.
6. Created an Azure Traffic Manager profile.
7. Added App Service endpoints to Traffic Manager.
8. Configured Traffic Manager health probes.
9. Validated traffic routing and automatic failover.

---

## ✅ Outcome
- Successfully deployed a web application across multiple Azure regions.
- Configured DNS-based traffic routing using Azure Traffic Manager.
- Implemented automatic failover between regions.
- Improved availability and resiliency of the application.
