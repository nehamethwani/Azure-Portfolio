# Project 6: Azure Monitoring & Alerts using Log Analytics and Data Collection Rules

## 📌 Overview
This project demonstrates implementing **enterprise‑grade monitoring** for an Azure Linux Virtual Machine using **Azure Monitor**, **Log Analytics**, and **Data Collection Rules (DCR)**. The focus of this project is on **operational visibility, telemetry ingestion, and alerting**, rather than application deployment.

The project reflects real‑world Azure environments where monitoring agents, telemetry pipelines, and governance controls play a critical role in observability.

---

## 🏗️ Architecture
Monitoring is implemented using Azure‑native services that collect metrics and logs from the virtual machine and centralize them in Log Analytics for analysis and alerting.

**Architecture flow:**

Linux Virtual Machine  
→ Azure Monitor Agent (AMA)  
→ Data Collection Rule (DCR)  
→ Log Analytics Workspace  
→ Metrics, Logs, and Alerts

---

## 🔧 Azure Services Used
- Azure Virtual Machine (Linux)
- Azure Monitor
- Azure Monitor Agent (AMA)
- Log Analytics Workspace
- Data Collection Rules (DCR)
- Azure Monitor Metrics & Alerts

---

## ⚙️ Implementation Steps (High Level)

### 1. Log Analytics Workspace
- Created a Log Analytics Workspace to act as a centralized log and metrics repository.
- Configured the workspace in the same region as the monitored resources, following Azure best practices.

### 2. Virtual Machine Creation
- Deployed a Linux Virtual Machine with Azure Monitor enabled at creation time.
- Enabled boot diagnostics and platform monitoring features.
- Verified VM availability and CPU/availability metrics via Azure Monitor.

### 3. Azure Monitor Agent
- Confirmed the Azure Monitor Linux Agent was successfully installed and running on the VM through Extensions.
- Validated agent provisioning status before proceeding with log ingestion configuration.

### 4. Data Collection Rule (DCR)
- Created a Data Collection Rule to explicitly define:
  - **What data to collect** (performance counters and Linux syslog)
  - **Where to send the data** (Log Analytics Workspace)
- Configured the DCR to collect:
  - Linux performance counters
  - Syslog entries (Info, Warning, Error)
- Associated the DCR with the target virtual machine.

### 5. Log Ingestion & Validation
- Verified telemetry ingestion using KQL queries in Log Analytics.
- Confirmed log and metric availability after DCR association and initial ingestion delay.
- Observed that syslog data is event‑driven and appears only when system activity occurs.

### 6. Monitoring & Alerts
- Used Azure Monitor metrics (CPU utilization, VM availability) for continuous monitoring.
- Created metric‑based alert rules (example: CPU utilization threshold) to trigger notifications.
- Validated alert configuration as part of proactive operational monitoring.

---

## 🔐 Security & Governance Considerations
- Monitoring configuration was implemented using least‑privilege access.
- Telemetry ingestion followed enterprise governance controls via Data Collection Rules.
- Agent‑based log ingestion required explicit DCR association, reflecting real‑world enterprise environments.
- Metrics‑based monitoring and Activity Logs were used alongside log analytics for comprehensive visibility.

---

## ✅ Outcome / Key Learnings
- Understood the difference between **metrics‑based monitoring** and **log‑based monitoring**
- Learned how Azure Monitor Agent depends on **Data Collection Rules** for telemetry ingestion
- Implemented a production‑style observability pipeline using Log Analytics and DCR
- Gained hands‑on experience with KQL queries for validating telemetry
- Experienced and documented enterprise governance impacts on monitoring behavior
- Built alerting mechanisms for proactive detection of performance issues

---

## 📌 Notes
Syslog data was observed to be event‑driven, appearing only after system activity, while performance metrics were sampled continuously. This behavior aligns with Azure Monitor’s design and is commonly seen in production environments.

This project emphasizes **operational readiness and observability**, which are critical components of real‑world cloud workloads.
``
