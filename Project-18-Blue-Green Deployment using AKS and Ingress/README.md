# Blue-Green Deployment on AKS with GitHub Actions CI/CD

## Overview

This project demonstrates a production-ready **Blue-Green Deployment Strategy** using:

- Azure Kubernetes Service (AKS)
- Azure Container Registry (ACR)
- NGINX Ingress Controller
- GitHub Actions CI/CD
- Docker
- Kubernetes

The solution enables **zero-downtime deployments** by maintaining two identical environments:

- **Blue Environment** → Current Production Version
- **Green Environment** → New Release Version

Traffic is switched to Green only after successful deployment validation and approval.

---

# Architecture

```text
Developer Push
      |
      v
GitHub Repository
      |
      v
GitHub Actions Pipeline
      |
      +-------------------+
      |                   |
      v                   v
Build Docker Image    Push to ACR
      |
      v
Deploy to Green Environment
      |
      v
Smoke Tests
      |
      v
Manual Approval
      |
      v
Switch Ingress Traffic
      |
      v
Production (Green)

Rollback:
Green ---> Blue
```

---

# Technology Stack

- Azure Kubernetes Service (AKS)
- Azure Container Registry (ACR)
- GitHub Actions
- Docker
- Kubernetes
- NGINX Ingress Controller
- Azure CLI
- Helm

---

# Project Structure

```text
bluegreen-aks/
│
├── app/
│   └── index.html
│
├── Dockerfile
│
├── k8s/
│   ├── namespace.yaml
│   ├── blue-deployment.yaml
│   ├── blue-service.yaml
│   ├── green-deployment.yaml
│   ├── green-service.yaml
│   └── ingress.yaml
│
├── .github/
│   └── workflows/
│       ├── bluegreen-deploy.yml
│       └── rollback.yml
│
└── README.md
```

---

# Prerequisites

Install the following tools:

```bash
az --version
kubectl version --client
helm version
docker --version
git --version
```

Required Accounts:

- Azure Subscription
- GitHub Account

---

# Azure Resources

The following Azure resources are used:

```text
Resource Group
|
├── AKS Cluster
|
├── Azure Container Registry
|
└── NGINX Ingress Controller
```

---

# Step 1: Create Resource Group

```bash
az group create \
--name rg-bluegreen-demo \
--location eastus
```

---

# Step 2: Create AKS Cluster

```bash
az aks create \
--resource-group rg-bluegreen-demo \
--name aks-bluegreen \
--node-count 2 \
--enable-managed-identity \
--generate-ssh-keys
```

Connect to AKS:

```bash
az aks get-credentials \
--resource-group rg-bluegreen-demo \
--name aks-bluegreen
```

Verify:

```bash
kubectl get nodes
```

---

# Step 3: Create Azure Container Registry

```bash
az acr create \
--resource-group rg-bluegreen-demo \
--name myacrbluegreen \
--sku Basic
```

Get Login Server:

```bash
az acr show \
--name myacrbluegreen \
--query loginServer \
--output tsv
```

Example:

```text
myacrbluegreen.azurecr.io
```

---

# Step 4: Attach ACR to AKS

```bash
az aks update \
--resource-group rg-bluegreen-demo \
--name aks-bluegreen \
--attach-acr myacrbluegreen
```

---

# Step 5: Install NGINX Ingress Controller

Add Helm Repository:

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

helm repo update
```

Install Ingress Controller:

```bash
helm install ingress-nginx ingress-nginx/ingress-nginx \
--namespace ingress-nginx \
--create-namespace
```

Verify:

```bash
kubectl get svc -n ingress-nginx
```

Copy the Public IP address.

---

# Step 6: Create Namespace

namespace.yaml

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: bluegreen
```

Apply:

```bash
kubectl apply -f namespace.yaml
```

---

# Step 7: Deploy Blue Environment

blue-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-blue
  namespace: bluegreen

spec:
  replicas: 2

  selector:
    matchLabels:
      version: blue

  template:
    metadata:
      labels:
        version: blue

    spec:
      containers:
      - name: app
        image: nginx:1.25
        ports:
        - containerPort: 80
```

Apply:

```bash
kubectl apply -f blue-deployment.yaml
```

---

# Step 8: Create Blue Service

blue-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: app-blue-svc
  namespace: bluegreen

spec:
  selector:
    version: blue

  ports:
  - port: 80
    targetPort: 80
```

Apply:

```bash
kubectl apply -f blue-service.yaml
```

---

# Step 9: Deploy Green Environment

green-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-green
  namespace: bluegreen

spec:
  replicas: 2

  selector:
    matchLabels:
      version: green

  template:
    metadata:
      labels:
        version: green

    spec:
      containers:
      - name: app
        image: IMAGE_PLACEHOLDER
        ports:
        - containerPort: 80
```

---

# Step 10: Create Green Service

green-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: app-green-svc
  namespace: bluegreen

spec:
  selector:
    version: green

  ports:
  - port: 80
    targetPort: 80
```

Apply:

```bash
kubectl apply -f green-service.yaml
```

---

# Step 11: Create Ingress

ingress.yaml

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  name: bluegreen-ingress
  namespace: bluegreen

spec:
  ingressClassName: nginx

  rules:
  - host: demo.contoso.com

    http:
      paths:
      - path: /
        pathType: Prefix

        backend:
          service:
            name: app-blue-svc

            port:
              number: 80
```

Apply:

```bash
kubectl apply -f ingress.yaml
```

---

# Step 12: Configure DNS

Map:

```text
demo.contoso.com
```

To:

```text
NGINX Public IP
```

Verify:

```bash
nslookup demo.contoso.com
```

---

# Step 13: Create Dockerfile

```dockerfile
FROM nginx:latest

COPY app/ /usr/share/nginx/html

EXPOSE 80
```

---

# Step 14: Create Azure Service Principal

GitHub Actions requires Azure authentication.

```bash
az ad sp create-for-rbac \
--name github-aks-sp \
--role Contributor \
--scopes /subscriptions/<subscription-id> \
--sdk-auth
```

Copy the output JSON.

---

# Step 15: Configure GitHub Secrets

Repository:

```text
Settings
    →
Secrets and Variables
    →
Actions
```

Create:

```text
AZURE_CREDENTIALS
ACR_NAME
ACR_LOGIN_SERVER
AKS_CLUSTER
AKS_RG
```

Example:

```text
AZURE_CREDENTIALS
```

Contains:

```json
{
  "clientId":"xxxxx",
  "clientSecret":"xxxxx",
  "subscriptionId":"xxxxx",
  "tenantId":"xxxxx"
}
```

---

# Step 16: Create GitHub Actions Workflow

File:

```text
.github/workflows/bluegreen-deploy.yml
```

Pipeline Flow:

```text
Push Code
    ↓
Build Docker Image
    ↓
Push Image to ACR
    ↓
Deploy to Green
    ↓
Smoke Test
    ↓
Manual Approval
    ↓
Switch Traffic Blue → Green
```

---

# CI/CD Workflow Stages

## Stage 1: Build

```text
Checkout Code
Azure Login
Build Docker Image
```

---

## Stage 2: Push

```text
Push Docker Image
to
Azure Container Registry
```

---

## Stage 3: Deploy Green

```text
Replace Image Placeholder
Deploy Green Pods
Deploy Green Service
```

---

## Stage 4: Smoke Test

Validate:

```text
Pod Health
Deployment Status
Application Reachability
```

Commands:

```bash
kubectl get pods -n bluegreen

kubectl rollout status deployment/app-green \
-n bluegreen
```

---

## Stage 5: Manual Approval

Create Environment:

```text
GitHub
  →
Settings
  →
Environments
  →
Production
```

Add:

```text
Required Reviewers
```

Pipeline pauses before production cutover.

---

## Stage 6: Switch Traffic

GitHub Actions executes:

```bash
kubectl patch ingress bluegreen-ingress \
-n bluegreen
```

Traffic:

```text
Blue
  ↓
Green
```

---

# Rollback Strategy

Create:

```text
.github/workflows/rollback.yml
```

Trigger:

```text
Actions
   →
Rollback Workflow
   →
Run Workflow
```

Rollback Traffic Flow:

```text
Green
   ↓
Blue
```

Expected rollback time:

```text
Less than 30 seconds
```

---

# Validation Commands

View Pods:

```bash
kubectl get pods -n bluegreen
```

View Services:

```bash
kubectl get svc -n bluegreen
```

View Ingress:

```bash
kubectl get ingress -n bluegreen
```

Describe Ingress:

```bash
kubectl describe ingress bluegreen-ingress \
-n bluegreen
```

View Logs:

```bash
kubectl logs deployment/app-green \
-n bluegreen
```

---

# End-to-End CI/CD Flow

```text
Developer Push
        ↓
GitHub Actions Trigger
        ↓
Build Docker Image
        ↓
Push to ACR
        ↓
Deploy Green Environment
        ↓
Smoke Testing
        ↓
Manual Approval
        ↓
Ingress Switch
        ↓
Production Traffic on Green
        ↓
Rollback Available
```

---

# Best Practices

- Use dedicated namespaces.
- Enable HTTPS using cert-manager.
- Enable Azure Monitor.
- Scan images using Trivy.
- Use commit SHA image tags.
- Use Readiness and Liveness Probes.
- Enable branch protection rules.
- Use GitHub Environments for production releases.
- Implement least-privilege RBAC.

---

# Future Enhancements

- Helm Deployment
- Azure Key Vault Integration
- Prometheus Monitoring
- Grafana Dashboards
- SonarQube Scans
- Trivy Security Scans
- Slack Notifications
- Microsoft Teams Notifications
- Terraform Infrastructure Deployment
- Canary Deployment Support

---

# Skills Demonstrated

- Azure Kubernetes Service (AKS)
- Azure Container Registry (ACR)
- Kubernetes Deployments
- Kubernetes Services
- Kubernetes Ingress
- Docker
- GitHub Actions
- Blue-Green Deployment
- CI/CD Automation
- Azure DevOps Concepts
- Rollback Strategy
- Zero-Downtime Deployment

---

