# Go Web Application - End-to-End DevOps & Cloud Deployment

![Go](https://img.shields.io/badge/Go-1.24-00ADD8?style=flat&logo=go)
![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?style=flat&logo=docker)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Orchestration-326CE5?style=flat&logo=kubernetes)
![AWS](https://img.shields.io/badge/AWS-EKS-FF9900?style=flat&logo=amazonaws)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-CI-2088FF?style=flat&logo=githubactions)
![Argo CD](https://img.shields.io/badge/Argo%20CD-GitOps-EF7B4D?style=flat&logo=argo)

## 📌 Project Overview

This project demonstrates an end-to-end DevOps workflow for a Go-based web application.

The goal of the project is to take a simple Go web application and build a complete cloud-native deployment process around it.

The application is first containerized using Docker and then deployed using Kubernetes. Kubernetes resources such as Deployments, Services, and Ingress are used to manage the application.

Continuous Integration is implemented using GitHub Actions. The CI pipeline automatically builds and tests the application and creates and publishes the Docker image.

Continuous Delivery is implemented using GitOps principles with Argo CD. The desired Kubernetes configuration is maintained in Git, and Argo CD synchronizes the Kubernetes cluster with the configuration stored in the repository.

The application is ultimately deployed to Amazon Elastic Kubernetes Service (EKS), which provides the managed Kubernetes environment on AWS.

Helm is used to package the Kubernetes application so that the same application can be deployed across multiple environments such as Development, QA, and Production using different configuration values.

NGINX Ingress Controller is used to handle incoming HTTP traffic and route requests to the Kubernetes application. A Load Balancer provides external access to the application, and DNS can be used to map a domain name to the application endpoint.

---

# 🎯 Project Objectives

The main objectives of this project are:

- Containerize a Go application using Docker.
- Use a multi-stage Docker build to create a smaller runtime image.
- Deploy the containerized application to Kubernetes.
- Create Kubernetes Deployment, Service, and Ingress resources.
- Understand Kubernetes networking and application exposure.
- Implement Continuous Integration using GitHub Actions.
- Build and publish Docker images automatically.
- Implement Continuous Delivery using GitOps.
- Use Argo CD to synchronize Kubernetes resources from Git.
- Create and configure an Amazon EKS cluster.
- Deploy the application to AWS.
- Package Kubernetes resources using Helm.
- Understand how Helm can support multiple environments.
- Configure an Ingress Controller.
- Expose the application through a Load Balancer.
- Understand DNS-based application access.
- Build an end-to-end DevOps workflow.

---

# 🏗️ High-Level Architecture

The project follows this overall workflow:

```text
                         DEVELOPER
                             |
                             v
                      GitHub Repository
                             |
              +--------------+--------------+
              |                             |
              v                             v
       GitHub Actions                    GitOps
              |                             |
              v                             v
        Build & Test                    Argo CD
              |                             |
              v                             v
        Docker Build                  Amazon EKS
              |                             |
              v                             v
         Docker Hub                  Kubernetes
                                            |
                               +------------+------------+
                               |            |            |
                               v            v            v
                           Deployment    Service      Ingress
                               |                         |
                               v                         v
                              Pods                Ingress Controller
                                                         |
                                                         v
                                                  Load Balancer
                                                         |
                                                         v
                                                        DNS
                                                         |
                                                         v
                                                       USER
