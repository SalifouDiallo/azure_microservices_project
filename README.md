# Cloud-Ready .NET Microservices Application

This project demonstrates the development, containerization, and cloud deployment of a .NET 9 microservices-based application using Azure and Kubernetes.

## 🔧 Technologies

- **.NET 9** – Microservices architecture (API, MVC, Workers)
- **Docker** – Containerization of services
- **Azure DevOps** – CI/CD pipelines
- **AKS (Azure Kubernetes Service)** – Orchestration platform
- **KEDA** – Autoscaling based on external triggers
- **Azure Key Vault** – Secure secret management
- **CosmosDB** – Scalable NoSQL database
- **Azure Event Hub / Service Bus** – Event-driven architecture
- **Azure Container Registry (ACR)** – Secure Docker image hosting
- **Azure Monitor** – Monitoring and logging
- **Azure App Configuration** – Centralized config management

## 🧩 Architecture Overview

The application consists of:
- An **API Gateway** for external access
- A **MVC Frontend** for user interaction
- Several **Worker services** for background processing
- All services communicate via HTTP and asynchronous messages (Event Hub / Service Bus)

Each component is containerized and deployed to AKS. KEDA dynamically scales the workers based on event volume.

## 🚀 CI/CD Workflow

Using **Azure DevOps Pipelines**:
1. **Build** Docker images for each microservice
2. **Push** images to **Azure Container Registry (ACR)**
3. **Deploy** to AKS using Kubernetes manifests and Helm charts
4. Apply autoscaling policies with KEDA

## 🔐 Security

- Secrets (DB strings, API keys) are stored and retrieved securely from **Azure Key Vault**
- Configuration settings are centralized using **Azure App Configuration**

## 📈 Monitoring & Testing

- Logs and metrics collected via **Azure Monitor**
- Load testing conducted using tools like **Apache JMeter** or **k6** to validate scaling performance

## 📸 Screenshots
![deploy-kube-infrastructure](https://github.com/user-attachments/assets/6b9fde7e-46e5-4487-b0d7-0ab5c127d7e2)
![create-infrastructure](https://github.com/user-attachments/assets/d2168762-ba73-4aee-82d9-b67d78166f38)
![Build and Publish![worker-imagesclaer](https://github.com/user-attachments/assets/937a8ada-5f15-44f0-a9d4-d1e269a5e9f7)
 Docker](https://github.com/user-attachments/assets/77a1cf13-085e-42f8-b856-2f7c0348ed9f)
![worker-content-x2](https://github.com/user-attachments/assets/ff7849f1-ee0b-4ec2-a485-e12d484bf7d4)


