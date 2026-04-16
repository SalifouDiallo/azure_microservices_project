# ☁️ Azure Microservices — Application cloud-native .NET 9

![.NET](https://img.shields.io/badge/.NET-9.0-512BD4?style=flat-square&logo=dotnet&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Conteneurisation-2496ED?style=flat-square&logo=docker&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-Cloud-0078D4?style=flat-square&logo=microsoftazure&logoColor=white)
![Kubernetes](https://img.shields.io/badge/AKS-Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white)
![CI/CD](https://img.shields.io/badge/Azure_DevOps-CI%2FCD-0078D4?style=flat-square&logo=azuredevops&logoColor=white)
![CosmosDB](https://img.shields.io/badge/CosmosDB-NoSQL-0078D4?style=flat-square&logo=microsoftazure&logoColor=white)

> Application cloud-native basée sur une architecture microservices en **.NET 9**, entièrement conteneurisée, déployée sur **Azure Kubernetes Service (AKS)** avec pipeline CI/CD automatisé via **Azure DevOps**.

---

## 📋 Table des matières

- [Architecture](#-architecture)
- [Technologies](#-technologies)
- [Structure du projet](#-structure-du-projet)
- [Pipeline CI/CD](#-pipeline-cicd)
- [Déploiement sur AKS](#-déploiement-sur-aks)
- [Sécurité](#-sécurité)
- [Supervision et tests](#-supervision-et-tests)
- [Captures du déploiement](#-captures-du-déploiement)

---

## 🏗️ Architecture

L'application est composée de **5 microservices indépendants** :

| Service | Rôle | Technologie |
|---|---|---|
| **API** | Gateway REST exposée à l'externe | .NET 9 Web API |
| **MVC** | Frontend utilisateur | .NET 9 MVC + Razor |
| **Worker_Content** | Traitement de contenu en arrière-plan | .NET 9 Worker |
| **Worker_DB** | Synchronisation base de données | .NET 9 Worker |
| **Worker_Image** | Traitement d'images | .NET 9 Worker |

Les services communiquent via **HTTP** et des **messages asynchrones** (Event Hub / Service Bus). KEDA ajuste dynamiquement le nombre de workers selon le volume d'événements.

```
                    ┌─────────────┐
                    │  API Gateway │
                    └──────┬──────┘
                           │ HTTP
              ┌────────────┼────────────┐
              ▼            ▼            ▼
         ┌─────────┐  ┌─────────┐  ┌─────────┐
         │  Worker │  │  Worker │  │  Worker │
         │ Content │  │   DB    │  │  Image  │
         └────┬────┘  └────┬────┘  └────┬────┘
              └────────────┼────────────┘
                           │ Event Hub / Service Bus
                    ┌──────▼──────┐
                    │  CosmosDB   │
                    └─────────────┘
```

---

## 🔧 Technologies

**Backend & Runtime**
- **.NET 9** — Architecture microservices (API, MVC, Workers)
- **Entity Framework Core** — ORM multi-provider (SQL, NoSQL, InMemory)

**Conteneurisation & Orchestration**
- **Docker** — Conteneurisation de chaque microservice
- **Docker Compose** — Environnement de développement local
- **AKS (Azure Kubernetes Service)** — Orchestration en production
- **KEDA** — Autoscaling basé sur des déclencheurs externes

**Cloud Azure**
- **Azure Event Hub** — Streaming d'événements à haute volumétrie
- **Azure Service Bus** — Messagerie asynchrone fiable
- **Azure CosmosDB** — Base de données NoSQL scalable
- **Azure Container Registry (ACR)** — Hébergement sécurisé des images Docker
- **Azure Key Vault** — Gestion sécurisée des secrets
- **Azure App Configuration** — Configuration centralisée
- **Azure Monitor** — Supervision et journalisation
- **Azure Blob Storage** — Stockage d'images et fichiers

**CI/CD**
- **Azure DevOps Pipelines** — 5 pipelines automatisés (build, test, deploy)
- **Helm Charts** — Déploiement Kubernetes déclaratif

---

## 📁 Structure du projet

```
azure_microservices_project/
├── API/                          # Microservice API Gateway
│   ├── Business/                 # Logique métier (EventHub)
│   ├── Data/                     # Repository pattern (SQL/NoSQL/InMemory)
│   ├── Models/                   # DTOs (Post, Comment)
│   ├── Dockerfile
│   └── Program.cs
├── MVC/                          # Microservice Frontend
│   ├── Business/                 # Blob, EventHub, ServiceBus, Telemetry
│   ├── Controllers/              # Posts, Comments, Home
│   ├── Data/                     # Repository pattern multi-provider
│   ├── Models/                   # Entités métier
│   ├── Views/                    # Razor Views
│   └── Dockerfile
├── Worker_Content/               # Worker traitement de contenu
├── Worker_DB/                    # Worker synchronisation BD
├── Worker_Image/                 # Worker traitement d'images
├── CloudInfrastructure/          # ARM Templates Azure
├── DeployACI/                    # Déploiement Azure Container Instances
├── DeployKube/                   # Manifestes Kubernetes + KEDA
│   ├── Cluster.yml               # Configuration du cluster AKS
│   └── keda-scaledobject.yml     # Règles d'autoscaling KEDA
├── IntegrationTesting/           # Tests d'intégration
├── azure-pipelines.yml           # Pipeline principal CI/CD
├── azure-pipelines-1..4.yml      # Pipelines spécialisés
└── docker-compose.yml            # Environnement local
```

---

## 🚀 Pipeline CI/CD

5 pipelines **Azure DevOps** automatisent le cycle de vie complet :

**Pipeline 1 — Build & Test**
```
Code Push → Build .NET → Tests unitaires → Analyse qualité
```

**Pipeline 2 — Conteneurisation**
```
Build Images Docker → Push vers Azure Container Registry (ACR)
```

**Pipeline 3 — Infrastructure**
```
Provision Azure Resources → AKS Cluster → CosmosDB → Event Hub
```

**Pipeline 4 — Déploiement AKS**
```
Pull depuis ACR → Deploy sur AKS → Apply KEDA ScaledObjects
```

**Pipeline 5 — Tests d'intégration**
```
Tests end-to-end → Validation des endpoints → Rapport de performance
```

---

## ☸️ Déploiement sur AKS

```bash
# Authentification Azure
az login
az aks get-credentials --resource-group <rg> --name <cluster>

# Déploiement du cluster
kubectl apply -f DeployKube/Cluster.yml

# Configuration de l'autoscaling KEDA
kubectl apply -f DeployKube/keda-scaledobject.yml

# Vérification des pods
kubectl get pods --all-namespaces
```

---

## 🔐 Sécurité

- **Azure Key Vault** — Secrets (chaînes de connexion, clés API) jamais codés en dur
- **Azure App Configuration** — Configuration centralisée et versionnée
- **Managed Identity** — Authentification sans mot de passe entre services Azure
- **Azure AD** — Authentification utilisateur intégrée

---

## 📈 Supervision et tests

- **Azure Monitor** — Collecte des logs et métriques de tous les microservices
- **Tests d'intégration** — Validation automatisée des endpoints via `IntegrationTesting/`
- **Tests de charge** — Apache JMeter / k6 pour valider la montée en charge et l'autoscaling KEDA

---

## 📸 Captures du déploiement

**Pipeline Azure DevOps — Exécution complète**
![Pipeline Azure DevOps](images/1.png)

**Création de l'infrastructure Azure**
![Infrastructure Azure](images/2.png)

**Packaging des microservices Docker**
![Docker Build](images/3.png)

**Déploiement et autoscaling sur AKS (KEDA)**
![AKS + KEDA](images/4.png)

---

## 👤 Auteur

**Salifou Diallo**  
Étudiant en informatique — UQAC  
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/salifou-diallo-3117702b2/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/SalifouDiallo)
