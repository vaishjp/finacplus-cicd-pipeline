#  FinacPlus CI/CD Pipeline Project

##  Overview

This project demonstrates a complete **CI/CD pipeline** implementation using **GitHub, Jenkins, Docker, and Kubernetes (Minikube)** on AWS EC2.

The pipeline automates the process of:

* Building application code
* Creating Docker images
* Pushing images to DockerHub
* Deploying updates to Kubernetes

 Goal: **Enable fast, consistent, and automated software delivery with minimal manual intervention.**

---

##  Architecture

GitHub (Code Repository)

Jenkins (CI/CD Pipeline)

Docker (Image Build & Push)

DockerHub (Image Registry)

Kubernetes - Minikube (Deployment)

---

##  Tech Stack

###  Cloud & Infrastructure

* AWS EC2 (Ubuntu)
* Minikube (Kubernetes cluster)

###  CI/CD

* Jenkins (Pipeline automation)
* Groovy (Jenkinsfile scripting)

###  Containerization

* Docker
* DockerHub (Image Registry)

###  Version Control

* Git
* GitHub

###  Application

* Node.js (Simple HTTP server)

---

##  CI/CD Pipeline Workflow

1. Developer pushes code to GitHub
2. Jenkins detects changes (Webhook / Poll SCM)
3. Jenkins pulls latest code
4. Docker image is built dynamically
5. Image is pushed to DockerHub
6. Kubernetes deployment is updated with new image
7. Application is redeployed automatically

---

##  Jenkinsfile (Pipeline Logic)

* Clone repository
* Build Docker image
* Authenticate with DockerHub
* Push image
* Update Kubernetes deployment

---

##  Setup Instructions

### 1️ EC2 Setup

* Launch Ubuntu EC2 instance
* Install Java, Docker, Git

### 2️ Jenkins Setup

* Install Jenkins using WAR file
* Access via port 8080
* Install required plugins

### 3️ Kubernetes Setup

* Install Minikube
* Start cluster
* Deploy sample application

### 4️ Docker Setup

* Build Docker image
* Push to DockerHub

### 5️ Jenkins Pipeline

* Create pipeline job
* Link GitHub repository
* Add credentials
* Configure triggers

---

##  Credentials Management

* DockerHub credentials stored securely in Jenkins
* GitHub access via Personal Access Token (PAT)
* Jenkins Credentials Manager used for secure authentication

---

##  Challenges Faced & Solutions

###  1. Jenkins Installation Issues

* Faced plugin installation failures
* Resolved by reinstalling Jenkins using WAR file

---

###  2. Docker Authentication Failure

* Error: unauthorized / incorrect credentials
* Solution: Used DockerHub **Access Token instead of password**

---

###  3. Docker Push Access Denied

* Cause: Repository not created / incorrect username
* Fix: Created correct repo and matched naming convention

---

###  4. Kubernetes Deployment Failure

* Error: container name mismatch
* Fix: Updated Jenkinsfile with correct container name

---

###  5. Networking Issues (Minikube on EC2)

* Service not accessible externally
* Fix: Used `kubectl proxy` and proper port exposure

---

###  6. Webhook Not Triggering

* Jenkins not reacting to GitHub events
* Fix: Configured SCM polling and GitHub integration

---

##  Improvements & Future Enhancements

###  1. Monitoring & Observability

* Integrate **Prometheus + Grafana**
* Track application health and performance

---

###  2. Use Managed Kubernetes (Production Ready)

* Replace Minikube with **AWS EKS or GKE**

---

###  3. Implement CI/CD Best Practices

* Add testing stage before deployment
* Use multi-stage Docker builds

---

###  4. Secure Pipeline

* Use secrets manager (AWS Secrets Manager / Vault)
* Avoid exposing credentials

---

###  5. Blue-Green / Canary Deployments

* Enable zero-downtime deployments

---

##  Key Learnings

* End-to-end CI/CD pipeline design
* Docker image lifecycle management
* Kubernetes deployment strategies
* Debugging real-world DevOps issues
* Importance of automation in software delivery

---

##  Conclusion

This project successfully demonstrates a **fully automated CI/CD pipeline** that integrates multiple DevOps tools to streamline application delivery.

 It ensures:

* Faster deployments
* Reduced manual effort
* Consistent and reliable releases

---

## Author

Vaishnavi J P

---

## Final Note

This project reflects practical DevOps implementation and problem-solving in a real-world scenario, covering deployment, automation, and debugging challenges.
