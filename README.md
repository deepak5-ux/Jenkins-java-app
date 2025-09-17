# Jenkins Pipeline for Java Application using Maven, SonarQube, Argo CD, and Kubernetes

![Pipeline Overview](https://user-images.githubusercontent.com/43399466/228301952-abc02ca2-9942-4a67-8293-f76647b6f9d8.png)

This document provides a step-by-step guide to set up an end-to-end Jenkins pipeline for a Java-based Spring Boot application. The pipeline integrates code quality checks with SonarQube, containerization with Docker, deployments with Helm and Kubernetes, and GitOps-based continuous delivery using Argo CD.

---

## Prerequisites

* Java application code hosted in a Git repository
* An **EC2 server** with the following installed:

  * Jenkins
  * SonarQube
  * Docker
  * Maven
* A **local Minikube cluster** with:

  * Kubernetes
  * Helm
  * Argo CD

---

## Step 1: Install Jenkins and SonarQube on EC2

### Jenkins Installation

```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

Access Jenkins at: `http://<EC2-Public-IP>:8080`

### SonarQube Installation

```bash
sudo apt update && sudo apt install unzip -y
adduser sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.4.1.88267.zip
unzip sonarqube-10.4.1.88267.zip
sudo mv sonarqube-10.4.1.88267 /opt/sonarqube
sudo chown -R sonarqube:sonarqube /opt/sonarqube
cd /opt/sonarqube/bin/linux-x86-64
./sonar.sh start
```

Access SonarQube at: `http://<EC2-Public-IP>:9000`

---

## Step 2: Install Required Jenkins Plugins

* Git Plugin
* Maven Integration Plugin
* Pipeline Plugin
* SonarQube Scanner Plugin
* Kubernetes Continuous Deploy Plugin

---

## Step 3: Configure Jenkins with Tools

1. Configure **Maven** under `Manage Jenkins > Global Tool Configuration`.
2. Configure **SonarQube server** under `Manage Jenkins > Configure System` with the SonarQube server URL and token.
3. Add **Docker** and **Kubernetes credentials** in Jenkins credentials.

---

## Step 4: Create Jenkins Pipeline

In Jenkins, create a new pipeline job and connect it to the application Git repository. Add a `Jenkinsfile` in the repository with the following stages:

### Pipeline Stages

1. **Checkout Code** – Fetch source from Git.
2. **Build Application** – Use Maven to compile and package.
3. **Run Tests** – Execute unit tests with JUnit/Mockito.
4. **SonarQube Analysis** – Static code analysis.
5. **Package Application** – Generate JAR file.
6. **Docker Build & Push** – Build and push image to Docker Hub.
7. **Deploy to Test (Helm)** – Deploy to Minikube using Helm.
8. **Run Acceptance Tests** – Validate deployment.
9. **Promote to Production (Argo CD)** – Sync to production namespace.

---

## Step 5: Set Up Minikube with Argo CD

### Install Argo CD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Expose Argo CD server:

```bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
kubectl get svc -n argocd
```

Access at: `http://<Minikube-IP>:<NodePort>`

### Connect Git Repository to Argo CD

* Add repository containing Helm charts and manifests.
* Create an Argo CD application pointing to the repo and target namespace.

---

## Step 6: Jenkins and Argo CD Integration

1. Generate Argo CD API token.
2. Add token to Jenkins credentials.
3. Update Jenkinsfile to include Argo CD deployment stage using API calls or CLI.

---

## Step 7: Run the Pipeline

1. Trigger the Jenkins pipeline manually or via a Git webhook.
2. Jenkins will:

   * Build the app with Maven.
   * Run tests and SonarQube analysis.
   * Build and push Docker image.
   * Deploy to Kubernetes test environment using Helm.
   * Sync production environment with Argo CD.

---

## Conclusion

This guide sets up an end-to-end CI/CD pipeline for a Java Spring Boot application using Jenkins, SonarQube, Docker, Helm, Kubernetes, and Argo CD. Jenkins and SonarQube run on an EC2 server, while Minikube hosts Kubernetes and Argo CD locally. This setup ensures automated builds, testing, code quality checks, containerization, and GitOps-driven deployments.

Users can replicate this setup on their own infrastructure by following the steps above.

