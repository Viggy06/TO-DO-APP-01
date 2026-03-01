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
- **Manifest Repository**: [TO-DO-APP-CONFIGS](https://github.com/Viggy06/TO-DO-APP-CONFIGS)  
  _(GitOps configuration and Kubernetes cluster manifests)_
- **Terraform Repository**: [Terraform-Infra-Repo](https://github.com/Viggy06/TO-DO-APP-Terraform)
  _(Infrastructure provisioning and remote state configuration)_
