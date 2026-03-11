## 🏗️ Architecture Overview

This project implements a complete **DevSecOps Lifecycle** for a **Three-Tier-Application**. It integrates infrastructure provisioning, automated security scanning, GitOps-driven deployments, and cluster observability.

### System Architecture

The workflow is divided into five functional layers:
* **Infrastructure:** AWS resources provisioned via **Terraform** with S3 remote state locking.
* **CI Pipeline:** **GitHub Actions** workflows featuring **Gitleaks** and **SonarQube** , **Trivy** for security.
* **Container Registry:** Secure image storage in **Amazon ECR**.
* **Continuous Deployment:** GitOps-based delivery using **ArgoCD** to sync manifests.
* **EKS Cluster:** A multi-tier app (Frontend/Backend/MongoDB) monitored by **Prometheus** and **Grafana**.

## Architecture

```mermaid
flowchart TB

%% =============================
%% Infrastructure Layer
%% =============================
subgraph Infrastructure
  Dev[Developer Laptop]
  TF[Terraform CLI]
  S3[S3 Remote State]
  Lock[S3 Lock File]

  Dev --> TF
  TF --> S3
  TF --> Lock
end

%% =============================
%% CI Pipeline
%% =============================
subgraph CI
  Repo[App Repository]
  GHA[GitHub Actions]
  Gitleaks
  TrivyFS[Trivy FS Scan]
  Build[Docker Build]
  TrivyImage[Trivy Image Scan]
  ECR[AWS ECR]

  Dev --> Repo
  Repo --> GHA
  GHA --> Gitleaks
  Gitleaks --> TrivyFS
  TrivyFS --> Build
  Build --> TrivyImage
  TrivyImage --> ECR
end

%% =============================
%% GitOps
%% =============================
subgraph GitOps
  ManifestRepo[Manifest Repository]
  ArgoCD
  Dev --> ManifestRepo
  ManifestRepo --> ArgoCD
end

%% =============================
%% EKS Cluster
%% =============================
subgraph EKS Cluster
  ALB
  Frontend
  Backend
  MongoDB
  Prometheus
  NodeExporter
  KSM

  ALB --> Frontend
  Frontend --> Backend
  Backend --> MongoDB

  NodeExporter --> Prometheus
  KSM --> Prometheus
end

%% =============================
%% Local Monitoring
%% =============================
subgraph Local Machine
  Grafana[Grafana Local]
end

%% Deployment Flow
ArgoCD --> Frontend
ArgoCD --> Backend
ArgoCD --> MongoDB
ECR --> Frontend
ECR --> Backend

%% Monitoring Flow
Prometheus --> Grafana
```
## Repository Visibility
- **Application Repository**: Public
- **Manifest Repository**: - Private (GitOps configuration and cluster details)
- **Terraform Repository**:  Private (infrastructure provisioning and state configuration)
