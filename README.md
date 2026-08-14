# Go Web App — End-to-End Cloud & DevOps Deployment

An end-to-end Cloud and DevOps project that takes a Go web application from local development to a containerized, Kubernetes-based, cloud-native deployment on **Amazon EKS**, with automated CI/CD using **GitHub Actions** and GitOps-based deployment using **Argo CD**.

The project demonstrates the complete application delivery lifecycle — from containerization and Kubernetes orchestration to cloud deployment, automation, and GitOps.

---

## 🚀 Project Overview

The project starts with a Go web application and progressively builds a production-oriented DevOps workflow around it.

### Deployment Flow

```text
Go Web Application
        │
        ▼
      Docker
        │
        ▼
   Docker Hub
        │
        ▼
    Kubernetes
        │
   ┌────┴─────┐
   ▼          ▼
Service    Ingress
              │
              ▼
       NGINX Ingress
              │
              ▼
          Helm
              │
              ▼
          AWS EKS
              │
       ┌──────┴──────┐
       ▼             ▼
GitHub Actions    Argo CD
    CI/CD          GitOps
```

---

## 🛠️ Technology Stack

### Application

* Go
* HTML/CSS

### Containerization

* Docker
* Docker Hub

### Kubernetes

* Kubernetes
* Deployments
* Services
* NGINX Ingress Controller
* Ingress
* Helm

### CI/CD & GitOps

* GitHub Actions
* CI/CD
* Argo CD
* GitOps

### Cloud

* AWS
* Amazon EKS
* AWS Load Balancing
* DNS

### Development Environment

* Linux / WSL
* Docker Desktop
* Git
* GitHub

---

## 📁 Project Structure

```text
go-web-app-cloud-devops/
│
├── main.go
├── go.mod
├── main_test.go
├── static/
│   ├── home.html
│   ├── about.html
│   ├── contact.html
│   ├── courses.html
│   └── images/
│
├── Dockerfile
├── .dockerignore
│
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
│
├── helm/
│   └── go-web-app/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│
├── .github/
│   └── workflows/
│       └── ...
│
├── argocd/
│   └── application.yaml
│
└── README.md
```

---

# 🐳 Docker Containerization

The Go application is containerized using a multi-stage Docker build.

The Docker build process:

1. Uses a Go image as the build environment.
2. Downloads application dependencies.
3. Compiles the Go application.
4. Creates a lightweight runtime image.
5. Copies the compiled application and required static files.
6. Exposes the application on port `8080`.

### Build the image

```bash
docker build -t go-web-app .
```

### Run the container

```bash
docker run -d -p 8080:8080 --name go-web-app go-web-app
```

The containerized application can then be accessed through:

```text
http://localhost:8080
```

The Docker image is also published to Docker Hub for Kubernetes deployment.

---

# ☸️ Kubernetes Deployment

The containerized application is deployed to Kubernetes using Kubernetes manifests.

The deployment consists of:

```text
Deployment
    │
    ├── Pod 1
    │
    └── Pod 2
```

Two replicas are used to provide basic availability and allow Kubernetes to distribute application traffic between multiple Pods.

### Deployment

```bash
kubectl apply -f k8s/deployment.yaml
```

Check the deployment:

```bash
kubectl get deployments
```

Check Pods:

```bash
kubectl get pods
```

---

# 🌐 Kubernetes Service

A Kubernetes Service provides stable networking for the application Pods.

```text
                Service
                   │
          ┌────────┴────────┐
          ▼                 ▼
       Pod 1              Pod 2
      :8080               :8080
```

The Service uses a selector to identify the Go application Pods.

```bash
kubectl apply -f k8s/service.yaml
```

Check the Service:

```bash
kubectl get services
```

---

# 🔀 NGINX Ingress

NGINX Ingress Controller is used as the entry point for HTTP traffic into the Kubernetes application.

The traffic flow is:

```text
Client
   │
   ▼
NGINX Ingress Controller
   │
   ▼
Ingress Resource
   │
   ▼
Kubernetes Service
   │
   ├──► Pod 1
   │
   └──► Pod 2
```

The Kubernetes Ingress resource defines the routing rules, while the NGINX Ingress Controller implements those rules.

```bash
kubectl apply -f k8s/ingress.yaml
```

---

# 📦 Helm

Helm is used to package and manage the Kubernetes application as a reusable Helm chart.

Instead of maintaining completely independent Kubernetes configurations, Helm allows deployment configuration to be parameterized through `values.yaml`.

Example:

```text
Helm Chart
    │
    ├── Deployment
    ├── Service
    └── Ingress
```

This makes it easier to manage different configurations and environments.

### Helm installation

```bash
helm install go-web-app ./helm/go-web-app
```

The deployment can be upgraded using:

```bash
helm upgrade go-web-app ./helm/go-web-app
```

---

# ☁️ AWS EKS Deployment

The application is deployed to **Amazon Elastic Kubernetes Service (EKS)**.

AWS CLI is configured from the terminal to authenticate with the AWS account and interact with AWS resources.

The application architecture on AWS is:

```text
User
 │
 ▼
DNS
 │
 ▼
AWS Load Balancer
 │
 ▼
Amazon EKS
 │
 ▼
NGINX Ingress
 │
 ▼
Kubernetes Service
 │
 ├──► Pod 1
 │
 └──► Pod 2
```

EKS provides the managed Kubernetes control plane while Kubernetes manages the application workloads.

---

# 🔄 CI/CD with GitHub Actions

GitHub Actions is used to automate the application delivery pipeline.

The CI/CD workflow automates tasks such as:

```text
Developer Push
      │
      ▼
GitHub Repository
      │
      ▼
GitHub Actions
      │
      ├── Build
      ├── Test
      ├── Docker Build
      └── Docker Image Push
             │
             ▼
         Docker Hub
```

This reduces manual steps involved in building and publishing new application versions.

---

# 🔁 GitOps with Argo CD

Argo CD is used to implement GitOps-based Kubernetes deployment.

The Git repository acts as the source of truth for the desired Kubernetes state.

```text
GitHub Repository
        │
        ▼
      Argo CD
        │
        ▼
    Amazon EKS
        │
        ▼
 Kubernetes Resources
```

When the desired configuration in Git changes, Argo CD synchronizes the Kubernetes environment with the repository state.

This provides declarative and automated application deployment.

---

# 🔐 Configuration & Security

The project follows basic DevOps security practices by:

* Keeping credentials outside application source code.
* Using GitHub Secrets for CI/CD credentials.
* Using AWS IAM for AWS authentication and authorization.
* Using Docker images rather than manually installing application dependencies on servers.
* Keeping Kubernetes configuration declarative and version controlled.

---

# 📊 Complete Architecture

```text
                       ┌─────────────────┐
                       │      User       │
                       └────────┬────────┘
                                │
                                ▼
                            DNS / URL
                                │
                                ▼
                       AWS Load Balancer
                                │
                                ▼
                       ┌─────────────────┐
                       │     AWS EKS     │
                       │                 │
                       │ NGINX Ingress   │
                       └────────┬────────┘
                                │
                                ▼
                       Kubernetes Service
                                │
                     ┌──────────┴──────────┐
                     ▼                     ▼
                  Pod 1                 Pod 2
                     │                     │
                     └──────────┬──────────┘
                                ▼
                         Go Application
```

### CI/CD and GitOps

```text
Developer
    │
    ▼
GitHub
    │
    ▼
GitHub Actions
    │
    ├── Test
    ├── Build
    ├── Docker Image
    └── Push Image
            │
            ▼
        Docker Hub

GitHub
    │
    ▼
  Argo CD
    │
    ▼
   EKS
    │
    ▼
Kubernetes
```

---

# 🎯 Key DevOps Concepts Demonstrated

* Containerization with Docker
* Multi-stage Docker builds
* Docker image management
* Kubernetes Deployments
* Kubernetes Services
* Kubernetes networking
* NGINX Ingress
* Helm-based deployments
* Amazon EKS
* AWS Load Balancing
* CI/CD with GitHub Actions
* GitOps with Argo CD
* Declarative infrastructure and application configuration
* Version-controlled deployment configuration

---

# 📌 Project Highlights

* Containerized a Go web application using Docker.
* Deployed the application with multiple Kubernetes replicas.
* Implemented Kubernetes Service-based networking.
* Configured NGINX Ingress for HTTP routing.
* Packaged Kubernetes resources using Helm.
* Deployed Kubernetes workloads to Amazon EKS.
* Automated application build and Docker image delivery using GitHub Actions.
* Implemented GitOps-based Kubernetes synchronization using Argo CD.
* Built an end-to-end cloud-native application delivery workflow.

---

## 👨‍💻 Author

**Himanshu Singh**

GitHub: **[@Himanshucodess](https://github.com/Himanshucodess)**
