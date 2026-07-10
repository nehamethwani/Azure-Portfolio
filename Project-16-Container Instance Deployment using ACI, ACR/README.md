# Azure Container Instance Deployment with Azure Container Registry

## Overview

This project demonstrates a complete container deployment workflow on Microsoft Azure. A simple Python Flask application is containerized using Docker, stored in Azure Container Registry (ACR), and deployed to Azure Container Instances (ACI). The solution enables fast and cost-effective deployment of containerized applications without managing virtual machines or orchestration platforms.

The project also includes an alternative deployment approach using Azure Cloud Shell for environments where corporate network policies prevent Docker image pushes to Azure Container Registry.

---

## Solution Architecture

```text
+-------------+      +-------------------+      +--------------------------+
| Developer   | ---> | Docker Container  | ---> | Azure Container Registry |
| Workstation |      | Build Process     |      | (ACR)                    |
+-------------+      +-------------------+      +--------------------------+
                                                       |
                                                       |
                                                       v
                                          +--------------------------+
                                          | Azure Container Instance |
                                          | (ACI)                    |
                                          +--------------------------+
                                                       |
                                                       |
                                                       v
                                          +--------------------------+
                                          | Public DNS Endpoint      |
                                          +--------------------------+
                                                       |
                                                       |
                                                       v
                                          +--------------------------+
                                          | End Users / Browser      |
                                          +--------------------------+
```

---

## Technologies Used

- Microsoft Azure
- Azure Container Registry (ACR)
- Azure Container Instances (ACI)
- Azure Resource Group
- Azure Cloud Shell
- Docker
- Python
- Flask
- Azure CLI

---

## Project Structure

```text
DockerProject/
│
├── app.py
├── requirements.txt
├── Dockerfile
└── README.md
```

---

## Application

### Flask Application

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Container deployed successfully on Azure ACI!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=80)
```

### Dependencies

```text
flask
```

### Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

EXPOSE 80

CMD ["python", "app.py"]
```

---

## Deployment Steps

### 1. Authenticate with Azure

```powershell
az login
```

### 2. Create a Resource Group

```powershell
az group create \
--name rg-container-demo \
--location eastus
```

### 3. Create Azure Container Registry

```powershell
az acr create \
--resource-group rg-container-demo \
--name acrcontainer123 \
--sku Basic
```

Retrieve the registry login server:

```powershell
az acr show \
--name acrcontainer123 \
--query loginServer \
--output tsv
```

### 4. Build the Docker Image

```powershell
docker build -t flaskapp:v1 .
```

Verify the image:

```powershell
docker images
```

### 5. Tag the Image

```powershell
docker tag flaskapp:v1 acrcontainer123.azurecr.io/flaskapp:v1
```

### 6. Push the Image to ACR

```powershell
az acr login --name acrcontainer123
```

```powershell
docker push acrcontainer123.azurecr.io/flaskapp:v1
```

Verify the repository:

```powershell
az acr repository list \
--name acrcontainer123 \
--output table
```

---

## Corporate Network Workaround

Some enterprise environments restrict Docker authentication and image pushes to Azure due to SSL inspection, proxy configurations, or firewall policies.

When this occurs, images can be built directly inside Azure using Cloud Shell.

Open Azure Cloud Shell and execute:

```bash
az acr build \
--registry acrcontainer123 \
--image flaskapp:v1 .
```

This eliminates the dependency on local Docker-to-ACR connectivity and performs the build securely within Azure services.

---

## Configure Registry Credentials

Enable the ACR admin account:

```powershell
az acr update \
--name acrcontainer123 \
--admin-enabled true
```

Retrieve credentials:

```powershell
az acr credential show \
--name acrcontainer123
```

---

## Deploy to Azure Container Instances

```powershell
az container create \
--resource-group rg-container-demo \
--name flaskaci \
--image acrcontainer123.azurecr.io/flaskapp:v1 \
--registry-login-server acrcontainer123.azurecr.io \
--registry-username <USERNAME> \
--registry-password <PASSWORD> \
--dns-name-label flaskaci-demo123 \
--ports 80
```

---

## Validation

Check deployment status:

```powershell
az container show \
--resource-group rg-container-demo \
--name flaskaci \
--query instanceView.state
```

Retrieve the public endpoint:

```powershell
az container show \
--resource-group rg-container-demo \
--name flaskaci \
--query ipAddress.fqdn \
--output tsv
```

Open the generated URL in a browser.

Expected response:

```text
Container deployed successfully on Azure ACI!
```

---

## Monitoring

View container logs:

```powershell
az container logs \
--resource-group rg-container-demo \
--name flaskaci
```

Attach to the running container:

```powershell
az container attach \
--resource-group rg-container-demo \
--name flaskaci
```

---

## Common Issues and Resolutions

### Docker Engine Not Running

Error:

```text
dockerDesktopLinuxEngine
The system cannot find the file specified
```

Resolution:

```powershell
docker ps
```

Ensure Docker Desktop is running and the Docker Engine is healthy.

### Authentication Failure

Error:

```text
401 Unauthorized
```

Resolution:

```powershell
az login
az acr login --name acrcontainer123
```

### SSL Connectivity Error

Error:

```text
CONNECTIVITY_SSL_ERROR
```

Resolution:

Use Azure Cloud Shell and build directly inside Azure:

```bash
az acr build \
--registry acrcontainer123 \
--image flaskapp:v1 .
```

---

## Cleanup

Delete the container instance:

```powershell
az container delete \
--resource-group rg-container-demo \
--name flaskaci \
--yes
```

Delete all deployed resources:

```powershell
az group delete \
--name rg-container-demo \
--yes
```

---

## Key Outcomes

- Containerized a Python Flask application using Docker.
- Managed private container images with Azure Container Registry.
- Deployed serverless containers using Azure Container Instances.
- Configured public DNS access for application consumption.
- Implemented monitoring and container log analysis.
- Utilized Azure Cloud Shell as an alternative deployment mechanism for restricted enterprise environments.
- Gained hands-on experience with end-to-end Azure container deployment workflows.
