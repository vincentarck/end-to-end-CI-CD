# I. Introduction

This project demonstrates a complete DevOps pipeline that automates the software delivery process from code commit to production deployment. We implement modern DevOps practices using industry-standard tools including Monitoring, Email notification, and RBAC setup for deployment.

## Project Overview

The pipeline automates these key processes:
- **Continuous Integration (CI)**
- **Continuous Deployment (CD)**
- **Containerization**
- **Orchestration**
- **Monitoring**

## Core Technologies

- **Version Control**: Git/GitHub
- **CI/CD**: Jenkins
- **Containerization**: Docker
- **Orchestration**: Kubernetes
- **Monitoring**: Prometheus & Grafana

## Key Features

1. Automated build triggers on code commits
2. Automated testing and quality checks
3. Container image building and registry storage
4. Automated deployment to Kubernetes with service and RBAC binding setup
5. Production-grade monitoring and alerting
6. Email notification with report from Trivy scanner

## Benefits

- Faster deployment cycles
- Reduced human error
- Consistent environments
- Scalable infrastructure
- Real-time monitoring

This project serves as both a learning resource and a template for implementing DevOps practices in real-world applications. You can find the sample app that we want to work with: [Sample App Repository](https://github.com/jaiswaladi246/Mission).

## Architecture Diagram

![image](https://github.com/user-attachments/assets/46404a0c-ced4-4f80-aa24-d6509c2f0374)


## Prerequisites & Tools Used

### 1. Jenkins Server
![jenkins-dashboard](https://github.com/user-attachments/assets/3f6f89fb-7af0-425f-910d-a149caf97510)
**Specifications:**
- **Machine Type**: n1-standard-2
- **RAM**: 8GB
- **vCPU**: 2 cores  
*(Equivalent to AWS t3.large)*

**Required Configurations:**
- Google Cloud SDK installation
- Kubernetes and Docker setup
- Maven authentication system

**Integration with:**
- SonarQube
- Nexus Repository
- Kubernetes Cluster

### 2. Nexus and SonarQube Server
![image](https://github.com/user-attachments/assets/4597f91b-db4b-4b02-9da6-1081e1b33068)
**Specifications:**
- **Machine Type**: n1-standard-1
- **RAM**: 4GB
- **vCPU**: 1 core

**Purpose:**
- Artifact repository management (Nexus)
- Code quality analysis (SonarQube)

### 3. Google Kubernetes Engine (GKE)

![image](https://github.com/user-attachments/assets/4c1be8e0-dbea-4bb3-93bf-3bd86c3e4532)

**Prerequisites:**
- Google Cloud SDK installation

**Required IAM Permissions:**
- Compute Admin: For creation of VM instances and nodes for your cluster
- Kubernetes Engine Admin: CRUD in CLI for your cluster
- Kubernetes Engine Cluster Admin: Kubernetes lifecycle management (creation of pods, services, RBAC, deployments, etc.)
- Service Account Admin: For easier debugging of IAM-related errors


### 4. Prometheus and Grafana
![prometheus-dashboard](https://github.com/user-attachments/assets/8fb0e8e6-5289-403f-ac3b-cb6a8bf06796)

## Thoughts
Since i used free credits in Google cloud i spent IDR 45k for running entire service in 3 days, only shutdown vm in nights and booting up again in morning, cluster running in all 3 days. While it's should more expensive since i get discount here 😂
![image](https://github.com/user-attachments/assets/13c0aca4-f044-4d68-ab3f-5c88a740bcb7)

![image](https://github.com/user-attachments/assets/58a226e3-a310-41ac-8936-25ec5ff9fc05)


