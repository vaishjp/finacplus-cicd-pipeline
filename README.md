# FinacPlus CI/CD Pipeline Project

## Overview

This project demonstrates a complete CI/CD pipeline implementation using GitHub, Jenkins, Docker, and Kubernetes (Minikube) on AWS EC2.

The pipeline automates:

- Building application code  
- Creating Docker images  
- Pushing images to DockerHub  
- Deploying updates to Kubernetes  

**Goal:** Enable fast, consistent, and automated software delivery with minimal manual intervention.

---

## Architecture
   GitHub (Code Repository)
  → Jenkins (CI/CD Pipeline)
  → Docker (Image Build & Push)
  → DockerHub (Image Registry)
  → Kubernetes - Minikube (Deployment)

  
---

## Tech Stack

### Cloud & Infrastructure
- AWS EC2 (Ubuntu)
- Minikube (Kubernetes cluster)

### CI/CD
- Jenkins (Pipeline automation)
- Groovy (Jenkinsfile scripting)

### Containerization
- Docker
- DockerHub

### Version Control
- Git
- GitHub

### Application
- Node.js (HTTP server)

---

## CI/CD Pipeline Workflow

1. Code is pushed to GitHub  
2. Jenkins detects changes (SCM Polling / Webhook)  
3. Jenkins pulls latest code  
4. Application is validated using test stage  
5. Docker image is built dynamically  
6. Image is pushed to DockerHub  
7. Kubernetes deployment is updated  
8. Deployment status is verified  

---

## Jenkins Pipeline Features

- Parameterized pipeline (supports multiple repositories and deployments)  
- Automated testing before build  
- Logging stage for traceability  
- Secure credential handling  
- Deployment verification using Kubernetes rollout  

---

## Setup Instructions

### 1. EC2 Setup
- Launch Ubuntu EC2 instance  
- Install Java, Docker, Git, Node.js  

### 2. Jenkins Setup
- Run Jenkins using WAR file  
- Install required plugins  
- Configure credentials  

### 3. Kubernetes Setup
- Install Minikube  
- Start cluster  
- Deploy application  

### 4. Pipeline Setup
- Create Jenkins pipeline job  
- Link GitHub repository  
- Configure triggers (Polling/Webhook)  

---

## Credentials Management

- DockerHub credentials stored securely in Jenkins  
- GitHub access via Personal Access Token (PAT)  
- No sensitive data hardcoded in scripts

## Test and Validation Strategy

- Application is validated using HTTP health check  
- Pipeline fails if application is not reachable  

**Deployment verified using:**
- `kubectl rollout status`  
- Ensures only working builds are deployed  

---

## Git Repository Configuration

* Jenkins is connected to GitHub using the repository URL.
* Authentication is handled using a Personal Access Token (PAT) stored securely in Jenkins credentials.
* The pipeline pulls code using:
    ```bash
    git clone
    ```

### Pipeline Triggering Mechanisms
* **SCM Polling:** Used to periodically check for changes.
* **GitHub Webhook:** Can be configured to trigger builds instantly on code push.

### Webhook Configuration
1.  Navigate to **GitHub repository → Settings → Webhooks**.
2.  Add the Jenkins endpoint:
    ```text
    http://<jenkins-ip>:8080/github-webhook/
    ```
3.  Set the trigger on **push events**.

---

## Kubernetes Cluster Configuration

* **Minikube** is used as the Kubernetes cluster running on the EC2 instance.
* The cluster is initialized using:
    ```bash
    minikube start
    ```
* Jenkins interacts with Kubernetes using:
    ```bash
    minikube kubectl
    ```
* A deployment is pre-created in Kubernetes for the application.
* The CI/CD pipeline updates the running application using:
    ```bash
    kubectl set image deployment/<deployment-name> <container-name>=<image>:<tag>
    ```
* Deployment verification is performed using:
    ```bash
    kubectl rollout status deployment/<deployment-name>
    ```

### Application Status and Debugging

* To check the status of the pods:
    ```bash
    kubectl get pods
    ```
* To view deployment logs:
    ```bash
    kubectl logs deployment/<deployment-name>
    ```
* Service exposure (if required):
    ```bash
    kubectl expose deployment <deployment-name> --type=NodePort --port=80
    ```

    -----

## Monitoring and Logging Strategy

- Jenkins console output used for pipeline monitoring  
- Logging stage provides build metadata  

**Kubernetes logs used for runtime debugging:**
- `kubectl logs deployment/my-app`  

**Deployment status checked using:**
- `kubectl get pods`  

---

## Scalability and Adaptability

**Pipeline is parameterized:**
- Repository URL  
- Docker image name  
- Deployment name  

**Can be reused for:**
- Multiple projects  
- Different Kubernetes clusters  

---

## Challenges Faced and Solutions

### 1. Jenkins Installation Issues
- Plugin failures during setup  
- Resolved by using Jenkins WAR installation  

### 2. Docker Authentication Failure
- Issue: Unauthorized login  
- Solution: Used DockerHub Access Token instead of password  

### 3. Docker Push Errors
- Issue: Repository mismatch  
- Solution: Corrected image naming and repository creation  

### 4. Kubernetes Deployment Failure
- Issue: Container name mismatch  
- Solution: Updated Jenkinsfile with correct container reference  

### 5. Minikube Networking Issues
- Issue: Service not accessible  
- Solution: Used proper port exposure and service commands  

### 6. Git Conflicts During Push
- Issue: Remote and local mismatch  
- Solution: Used `git pull --rebase` to maintain clean history  

### 7. Jenkins Pipeline Failure (Test Stage)
- Issue: Node.js not installed on server  
- Solution: Installed runtime dependencies and validated environment  

### 8. Groovy and Shell Integration Error
- Issue: “Bad substitution” error  
- Solution: Corrected variable interpolation using proper quoting  

---

## Improvements Implemented

- Added test validation stage before deployment  
- Added logging stage for better observability  
- Implemented deployment verification  
- Parameterized pipeline for scalability  
- Improved error handling and debugging visibility

---
## Running the Project (Live Demo Steps)

### 1. Connect to EC2

    ssh -i "C:\Users\User\Downloads\finacplus-key.pem.pem" ubuntu@13.234.19.110

### 2. Navigate to Project

    cd ~/my-app

### 3. Start Required Services

#### Start Docker

    sudo systemctl start docker

#### Start Kubernetes (Minikube)

    minikube start --driver=docker --memory=3000mb --cpus=2

---

### 4. Verify Kubernetes Cluster

    minikube kubectl -- get nodes
    minikube kubectl -- get pods

---

### 5. Start Jenkins

    cd ~
    java -jar jenkins.war --httpPort=8080

---

### 6. Access Jenkins Dashboard

Open in browser:

    http://13.234.19.110:8080

---

### 7. Run CI/CD Pipeline

- Open pipeline job  
- Click **Build Now**

**Monitor stages:**

- Clone  
- Test  
- Build  
- Push  
- Deploy  
- Verify  

---

### 8. Verify Deployment

    minikube kubectl -- get pods
    minikube kubectl -- get svc

---

### 9. View Application Logs

    minikube kubectl -- logs deployment/my-app

---

### 10. Trigger Pipeline via Git (Automation Demo)

    cd ~/my-app
    echo "trigger $(date)" >> test.txt
    git add .
    git commit -m "trigger pipeline"
    git pull origin main --rebase
    git push

---

### 11. Troubleshooting

#### Check if Jenkins is running

    ps aux | grep jenkins

#### Restart Jenkins if needed

    sudo pkill -f jenkins
    java -jar ~/jenkins.war --httpPort=8080

---

## Future Enhancements

- Integrate Prometheus and Grafana for monitoring  
- Use AWS EKS instead of Minikube  
- Implement rollback strategies  
- Add automated test frameworks  
- Introduce blue-green deployment  

---

## Key Learnings

- End-to-end CI/CD pipeline design  
- Docker image lifecycle management  
- Kubernetes deployment strategies  
- Real-world debugging and troubleshooting  
- Importance of automation and validation  

---

## Live Demo Steps

1. Connect to EC2  
2. Start Docker  
3. Start Minikube  
4. Start Jenkins  
5. Run pipeline  
6. Verify deployment using Kubernetes  
7. Trigger pipeline via Git commit  

---

## Conclusion

This project demonstrates a fully automated CI/CD pipeline integrating GitHub, Jenkins, Docker, and Kubernetes.

It ensures:

- Faster deployments  
- Reduced manual effort  
- Consistent and reliable releases  

---

## Author

**Vaishnavi J P**









