# Project 2: Linux Virtual Machine Web Hosting on Azure

## 📌 Overview
This project demonstrates hosting a website on an Azure Linux Virtual Machine using the Nginx web server. The objective of this project is to understand Infrastructure as a Service (IaaS) concepts, VM-based hosting, and enterprise-grade security and networking practices in Azure.

---

## 🏗️ Architecture
User traffic is routed securely to a Linux Virtual Machine that hosts a web application using Nginx, while administrative access is restricted using Azure Bastion.

Architecture flow:
User → Azure Bastion → Linux VM → Nginx Web Server → Website Content

---

## 🔧 Azure Services Used
- Azure Virtual Machine (Linux)
- Azure Virtual Network (VNet)
- Network Security Group (NSG)
- Azure Bastion
- Azure NAT Gateway
- Nginx Web Server

---

## ⚙️ Implementation Steps (High Level)
1. Created a virtual network with required subnets
2. Deployed a Linux Virtual Machine without a public IP
3. Configured Network Security Groups to control traffic
4. Enabled secure VM access using Azure Bastion
5. Installed and configured Nginx on the Linux VM
6. Hosted website files on the VM
7. Provided controlled outbound internet access using NAT Gateway
8. Verified website functionality from within the private network

---

## 🔐 Security & Governance Considerations
- No public IP assigned to the virtual machine
- Administrative access restricted via Azure Bastion
- Inbound traffic controlled using Network Security Groups
- Outbound traffic regulated using NAT Gateway
- Deployment aligned with enterprise security policies and governance constraints

---

## ✅ Outcome / Key Learnings
- Gained hands-on experience with Azure IaaS
- Learned Linux virtual machine provisioning and management
- Understood secure VM access using Bastion
- Implemented enterprise networking concepts such as NAT and NSGs
- Compared VM-based hosting with PaaS alternatives like Azure App Service
- Experienced real-world Azure policy and security restrictions

---

## 📌 Notes
This project intentionally avoids direct public exposure of the VM to reflect real-world enterprise cloud architectures where security and controlled access are prioritized.
