# AKS Microservices Deployment using Kubernetes & Helm

## Project Overview

This project demonstrates the deployment of a containerized microservices application on Azure Kubernetes Service (AKS) using Docker, Kubernetes, Helm, and Azure Container Registry (ACR).

The solution showcases cloud-native application deployment, container orchestration, scalability, service discovery, load balancing, and release management in Azure.

---

## Architecture

images/aks-microservices-architecture.png

---

## Architecture Flow

```text
User
  │
  ▼
Azure Load Balancer
  │
  ▼
AKS Cluster
  │
  ├── Frontend Service
  │
  ├── User Service
  │
  └── Product Service
          │
          ▼
      Database
```

Container images are stored in Azure Container Registry (ACR), deployed to AKS using Kubernetes manifests and managed through Helm Charts.

---

## Technologies Used

- Microsoft Azure
- Azure Kubernetes Service (AKS)
- Azure Container Registry (ACR)
- Docker
- Kubernetes
- Helm
- Azure CLI
- Kubectl

---

## Project Structure

```text
AKS-Microservices/
│
├── frontend/
│   ├── app.py
│   └── Dockerfile
│
├── userservice/
│   ├── app.py
│   └── Dockerfile
│
├── productservice/
│   ├── app.py
│   └── Dockerfile
│
├── kubernetes/
│   ├── deployment.yaml
│   └── service.yaml
│
├── helm-chart/
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
│
├── images/
│   └── aks-microservices-architecture.png
│
└── README.md
```

---

# Step 1: Create Azure Resource Group

```bash
az group create \
--name rg-aks-microservices \
--location eastus
```

---

# Step 2: Create Azure Container Registry

```bash
az acr create \
--resource-group rg-aks-microservices \
--name aksmicroacr \
--sku Standard
```

---

# Step 3: Create AKS Cluster

```bash
az aks create \
--resource-group rg-aks-microservices \
--name aks-cluster \
--node-count 2 \
--generate-ssh-keys
```

---

# Step 4: Connect to AKS

```bash
az aks get-credentials \
--resource-group rg-aks-microservices \
--name aks-cluster
```

Verify:

```bash
kubectl get nodes
```

---

# Step 5: Attach ACR to AKS

```bash
az aks update \
--resource-group rg-aks-microservices \
--name aks-cluster \
--attach-acr aksmicroacr
```

---

# Sample Microservice

## app.py

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "User Service Running"

app.run(host="0.0.0.0", port=5000)
```

---

## requirements.txt

```text
flask
```

---

## Dockerfile

```dockerfile
FROM python:3.10

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

CMD ["python","app.py"]
```

---

# Build Docker Image

```bash
docker build -t userservice:v1 .
```

Tag image:

```bash
docker tag userservice:v1 \
aksmicroacr.azurecr.io/userservice:v1
```

Push image:

```bash
docker push aksmicroacr.azurecr.io/userservice:v1
```

---

# Kubernetes Deployment

## deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: userservice

spec:
  replicas: 2

  selector:
    matchLabels:
      app: userservice

  template:
    metadata:
      labels:
        app: userservice

    spec:
      containers:
      - name: userservice
        image: aksmicroacr.azurecr.io/userservice:v1

        ports:
        - containerPort: 5000
```

Apply deployment:

```bash
kubectl apply -f deployment.yaml
```

---

# Kubernetes Service

## service.yaml

```yaml
apiVersion: v1
kind: Service

metadata:
  name: userservice

spec:
  selector:
    app: userservice

  ports:
  - port: 80
    targetPort: 5000

  type: ClusterIP
```

Apply service:

```bash
kubectl apply -f service.yaml
```

---

# Verify Resources

```bash
kubectl get pods
kubectl get deployments
kubectl get services
```

Example Output:

```text
NAME                          READY   STATUS
userservice-85f98dbd-rjbc9    1/1     Running
userservice-85f98dbd-j4trp    1/1     Running
```

---

# Create Helm Chart

Generate chart:

```bash
helm create microservices-chart
```

---

## Chart.yaml

```yaml
apiVersion: v2
name: microservices-chart
description: AKS Microservices Deployment
version: 0.1.0
appVersion: "1.0"
```

---

## values.yaml

```yaml
replicaCount: 2

image:
  repository: aksmicroacr.azurecr.io/userservice
  tag: v1
```

---

## Deployment Template

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: {{ .Release.Name }}

spec:
  replicas: {{ .Values.replicaCount }}

  selector:
    matchLabels:
      app: {{ .Release.Name }}

  template:
    metadata:
      labels:
        app: {{ .Release.Name }}

    spec:
      containers:
      - name: {{ .Release.Name }}
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
```

---

# Deploy Using Helm

Install chart:

```bash
helm install userservice ./microservices-chart
```

Check release:

```bash
helm list
```

---

# Upgrade Deployment

Update image version:

```yaml
tag: v2
```

Upgrade:

```bash
helm upgrade userservice ./microservices-chart
```

---

# Rollback Deployment

```bash
helm rollback userservice 1
```

---

# Horizontal Pod Autoscaling

Create HPA:

```bash
kubectl autoscale deployment userservice \
--cpu-percent=70 \
--min=2 \
--max=10
```

Verify:

```bash
kubectl get hpa
```

---

# Monitoring

Install Prometheus

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

```bash
helm repo update
```

```bash
helm install prometheus prometheus-community/kube-prometheus-stack
```

Verify:

```bash
kubectl get pods
```

---

# Project Outcomes

- Successfully deployed microservices on Azure Kubernetes Service.
- Containerized applications using Docker.
- Stored images in Azure Container Registry.
- Implemented Kubernetes Deployments and Services.
- Managed deployments using Helm Charts.
- Enabled application scaling using Horizontal Pod Autoscaler.
- Demonstrated production-ready container orchestration on Azure.
- Implemented release upgrades and rollback strategies.

---

# Learning Outcomes

Through this project, I gained hands-on experience with:

- Azure Kubernetes Service (AKS)
- Kubernetes Architecture
- Docker Containerization
- Azure Container Registry (ACR)
- Helm Package Management
- Kubernetes Networking
- Container Orchestration
- Application Scaling
- Cloud-native Deployments
- Azure DevOps Practices

---

# Future Enhancements

- CI/CD using GitHub Actions
- Azure DevOps Pipelines
- NGINX Ingress Controller
- SSL/TLS Configuration
- Azure Key Vault Integration
- Prometheus & Grafana Dashboards
- Multi-Environment Deployments
- Service Mesh using Istio

