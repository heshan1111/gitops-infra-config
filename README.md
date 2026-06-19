# Production-Grade GitOps CI/CD Pipeline - Infrastructure Configuration Repository

This repository functions as the Single Source of Truth (SSoT) for the cluster state in an advanced declarative GitOps pipeline. It manages Kubernetes manifests and orchestrates fully automated Continuous Delivery (CD) using ArgoCD onto a local Kubernetes Cluster.

---

## Tech Stack & Key Features

* Orchestration: Local Kubernetes Cluster managed via Docker Desktop.
* GitOps Engine: ArgoCD tracking application specifications and repository configurations natively.
* Traffic Routing: Kubernetes Services configured as a LoadBalancer executing traffic management over standard web port 80.
* High Availability: Declarative replica management with automatic self-healing capabilities.

---

## Deployment & Scaling Topology

1. GitHub Config Repo: Tracks environment configuration updates continuously.
2. ArgoCD GitOps Engine: Automates Synchronization and executes Drift Detection.
3. Kubernetes Cluster: Deploys manifests dynamically inside the host architecture.
4. LoadBalancer Service: Exposes and balances web traffic on port 80.
5. Replicated Pods: Ensures high availability and native auto-healing.

---

## How to Deploy via ArgoCD

### 1. Initialize ArgoCD on Kubernetes

Apply the official ArgoCD manifests to your local cluster environment:
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

### 2. Access the ArgoCD Dashboard

Expose the ArgoCD API Server locally:
kubectl port-forward svc/argocd-server -n argocd 8080:443

* URL: https://localhost:8080
* Username: admin
* Fetch Encrypted Password:
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"

Note: Decode the resulting base64 string in your PowerShell or Terminal to capture the raw credential.

### 3. Application Specification Manifest Configuration

Register the cluster configuration into ArgoCD with the following parameters:
* Application Name: fastapi-gitops-app
* Project: default
* Repository URL: https://github.com/heshan1111/gitops-infra-config.git
* Path: .
* Cluster URL: https://kubernetes.default.svc
* Namespace: default

---

## High Availability & Declarative Scaling

This project utilizes advanced declarative scaling configurations. Modifying the replicas key within deployment.yaml (e.g., from 2 to 3) and committing changes directly to GitHub triggers instant Drift Detection inside ArgoCD. 

Clicking Synchronize forces the cluster topology to live-scale instantly with zero downtime, initializing new working application pods through an automated visual interface.
