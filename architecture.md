# 📌 Project Architecture

This document describes the architecture of the Guest Registration System, including its key components and their interactions.

## 🔹 System Architecture Diagram

```mermaid
graph TD
    Developer --> |Develops code| DevContainer
    DevContainer -->|Pushes code| GitHubRepository
    GitHubRepository -->|Triggers| GitHubActions
    GitHubActions -->|Builds & Deploys App & Infra| Azure
    GitHubActions -->|Publishes| GitHubPackages
    AzureContainerApps -->|Stores data| AzureCosmosDB[(AzureCosmosDB)]
    User -->|Uses app| AzureContainerApps
    DevContainer o-.-o |Pulls images| GitHubPackages
    AzureContainerApps o-.-o |Pulls images| GitHubPackages

    subgraph Local Environment
        Developer
        DevContainer
    end

    subgraph GitHub
        GitHubRepository
        GitHubActions
        GitHubPackages
    end

    subgraph Azure
        AzureContainerApps
        AzureCosmosDB
    end

    subgraph GoogleCloud[Google Cloud Platform]
        GoogleCloudAPIGateway["Google Cloud API Gateway"]
    end

    subgraph ExternalAPIs
        AirbnbAPI["Airbnb API"]
        BuildingAPI["Building API"]
    end

    AzureContainerApps -->|Routes API Calls| GoogleCloudAPIGateway
    GoogleCloudAPIGateway -->|Pushess guest data| AzureContainerApps
    GoogleCloudAPIGateway -->|Fetches guest data| AirbnbAPI
    GoogleCloudAPIGateway -->|Pushes guest data| BuildingAPI

```

## 🏗️ Architecture Components

- **Frontend:** A Vue.js-based application hosted on **Azure Static Web Apps**, allowing guests to register their details.
- **API Gateway:** **Google Cloud API Gateway** handles requests, integrates with external APIs.
- **Backend:** An API deployed on **Azure Container Apps**, handling form submissions and retrieving guest data.
- **Database:** **Azure Cosmos DB** is used to store guest information in a scalable, serverless manner.
- **CI/CD Pipeline:** GitHub Actions automates testing, linting, and deployment of the application.