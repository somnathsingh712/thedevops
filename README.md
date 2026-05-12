# Complete DevOps Project using Node.js on AWS EC2

# Project Title

Complete DevOps Lifecycle Implementation using Node.js, Git, Docker, Jenkins, Kubernetes, Prometheus, and Grafana on AWS EC2

---

# Project Objective

The objective of this project is to implement a complete DevOps workflow using a simple Node.js application deployed on an AWS EC2 virtual machine. The project demonstrates:

* Version control using Git and GitHub
* Virtualization using AWS EC2
* Containerization using Docker
* CI/CD pipeline using Jenkins
* Container orchestration using Kubernetes (Minikube)
* Monitoring using Prometheus
* Visualization using Grafana

---

# Technologies Used

| Technology   | Purpose                        |
| ------------ | ------------------------------ |
| AWS EC2      | Virtual Machine / Cloud Server |
| Ubuntu       | Operating System               |
| Git & GitHub | Version Control                |
| Node.js      | Application Runtime            |
| Docker       | Containerization               |
| Jenkins      | CI/CD Automation               |
| Kubernetes   | Container Orchestration        |
| Minikube     | Local Kubernetes Cluster       |
| Helm         | Kubernetes Package Manager     |
| Prometheus   | Monitoring Tool                |
| Grafana      | Visualization Dashboard        |

---

# Project Architecture

```text
GitHub Repository
        ↓
Jenkins Pipeline
        ↓
Docker Image Build
        ↓
Kubernetes Deployment
        ↓
Node.js Application
        ↓
Prometheus Monitoring
        ↓
Grafana Dashboard Visualization
```

---

# EC2 Instance Configuration

| Configuration    | Value        |
| ---------------- | ------------ |
| Instance Type    | t2.medium    |
| Operating System | Ubuntu 22.04 |
| Storage          | 25 GB        |

---

# Security Group Inbound Rules

| Port  | Purpose             |
| ----- | ------------------- |
| 22    | SSH Access          |
| 3000  | Node.js Application |
| 3001  | Grafana             |
| 8080  | Jenkins             |
| 9090  | Prometheus          |
| 30007 | Kubernetes NodePort |

Source:

```text
0.0.0.0/0
```

---

# Step 1: Connect to EC2 Instance

```bash
ssh -i thedev.pem ubuntu@YOUR_PUBLIC_IP
```

---

# Step 2: Update Ubuntu Packages

```bash
sudo apt update && sudo apt upgrade -y
```

---

# Step 3: Install Git

```bash
sudo apt install git -y
```

Verify:

```bash
git --version
```

---

# Step 4: Install Node.js

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```

```bash
sudo apt install nodejs -y
```

Verify:

```bash
node -v
npm -v
```

---

# Step 5: Install Docker

```bash
sudo apt install docker.io -y
```

Enable Docker:

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

Add Ubuntu user to Docker group:

```bash
sudo usermod -aG docker ubuntu
```

Apply group changes:

```bash
newgrp docker
```

Verify:

```bash
docker --version
```

---

# Step 6: Create Node.js Application

Create project folder:

```bash
mkdir node-devops-project
cd node-devops-project
```

Initialize project:

```bash
npm init -y
```

Install Express:

```bash
npm install express
```

Create app.js:

```bash
nano app.js
```

Paste:

```javascript
const express = require('express');

const app = express();

app.get('/', (req, res) => {
    res.send('Node.js DevOps Project Running Successfully');
});

app.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

Run application:

```bash
node app.js
```

Open browser:

```text
http://YOUR_PUBLIC_IP:3000
```

---

# Step 7: Git and GitHub Integration

Initialize Git:

```bash
git init
```

Add files:

```bash
git add .
```

Commit files:

```bash
git commit -m "Initial commit"
```

Rename branch:

```bash
git branch -M main
```

Connect GitHub repository:

```bash
git remote add origin YOUR_GITHUB_REPO_URL
```

Push code:

```bash
git push -u origin main
```

---

# Step 8: Docker Containerization

Create Dockerfile:

```bash
nano Dockerfile
```

Paste:

```dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "app.js"]
```

Build Docker image:

```bash
docker build -t node-devops-app .
```

Run Docker container:

```bash
docker run -d -p 3000:3000 --name node-container node-devops-app
```

Verify:

```bash
docker ps
```

---

# Step 9: Install Java 21

```bash
sudo apt install openjdk-21-jdk -y
```

Verify:

```bash
java -version
```

---

# Step 10: Install Jenkins

Add Jenkins repository:

```bash
echo "deb [trusted=yes] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list
```

Update packages:

```bash
sudo apt update
```

Install Jenkins:

```bash
sudo apt install jenkins -y
```

Enable Jenkins:

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

Check status:

```bash
sudo systemctl status jenkins
```

Get Jenkins password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Open Jenkins:

```text
http://YOUR_PUBLIC_IP:8080
```

---

# Step 11: Configure Jenkins Docker Permission

```bash
sudo usermod -aG docker jenkins
```

Restart Jenkins:

```bash
sudo systemctl restart jenkins
```

---

# Step 12: Jenkins Pipeline Configuration

Create Jenkins pipeline job:

* Open Jenkins
* New Item
* Pipeline
* OK

Choose:

```text
Pipeline Script
```

Paste:

```groovy
pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t node-devops-app .'
            }
        }

        stage('Stop Existing Container') {
            steps {
                sh 'docker stop node-container || true'
                sh 'docker rm node-container || true'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 3000:3000 --name node-container node-devops-app'
            }
        }
    }
}
```

Run pipeline using:

```text
Build Now
```

---

# Step 13: Install Kubernetes Tools

Install kubectl:

```bash
curl -LO "https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl"
```

```bash
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

Install Minikube:

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```

```bash
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

Start Minikube:

```bash
minikube start --driver=docker
```

Verify:

```bash
kubectl get nodes
```

---

# Step 14: Kubernetes Deployment

Create deployment.yaml:

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: node-deployment

spec:
  replicas: 2

  selector:
    matchLabels:
      app: node-app

  template:
    metadata:
      labels:
        app: node-app

    spec:
      containers:
      - name: node-app
        image: node-devops-app:latest
        imagePullPolicy: Never

        ports:
        - containerPort: 3000
```

---

# Step 15: Kubernetes Service

Create service.yaml:

```yaml
apiVersion: v1
kind: Service

metadata:
  name: node-service

spec:
  type: NodePort

  selector:
    app: node-app

  ports:
  - port: 3000
    targetPort: 3000
    nodePort: 30007
```

---

# Step 16: Build Docker Image Inside Minikube

Connect Docker to Minikube:

```bash
eval $(minikube docker-env)
```

Build image:

```bash
docker build -t node-devops-app:latest .
```

---

# Step 17: Deploy Application to Kubernetes

Apply deployment:

```bash
kubectl apply -f deployment.yaml
```

Apply service:

```bash
kubectl apply -f service.yaml
```

Verify:

```bash
kubectl get pods
kubectl get svc
```

---

# Step 18: Access Kubernetes Application

Port forward application:

```bash
kubectl port-forward service/node-service 3000:3000 --address 0.0.0.0
```

Open browser:

```text
http://YOUR_PUBLIC_IP:3000
```

---

# Step 19: Install Helm

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

Verify:

```bash
helm version
```

---

# Step 20: Install Prometheus

Add Helm repository:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

Update repositories:

```bash
helm repo update
```

Install Prometheus:

```bash
helm install prometheus prometheus-community/prometheus
```

---

# Step 21: Access Prometheus

Port forward:

```bash
kubectl port-forward svc/prometheus-server 9090:80 --address 0.0.0.0
```

Open browser:

```text
http://YOUR_PUBLIC_IP:9090
```

Query:

```text
up
```

Result value:

```text
1
```

indicates healthy monitoring targets.

---

# Step 22: Install Grafana

Add Grafana repository:

```bash
helm repo add grafana https://grafana.github.io/helm-charts
```

Update repositories:

```bash
helm repo update
```

Install Grafana:

```bash
helm install grafana grafana/grafana
```

---

# Step 23: Access Grafana

Get Grafana password:

```bash
kubectl get secret grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

Port forward Grafana:

```bash
kubectl port-forward svc/grafana 3001:80 --address 0.0.0.0
```

Open browser:

```text
http://YOUR_PUBLIC_IP:3001
```

Login credentials:

| Username | Password            |
| -------- | ------------------- |
| admin    | Output from command |

---

# Step 24: Add Prometheus Data Source in Grafana

Inside Grafana:

```text
Connections
→ Data Sources
→ Add Data Source
→ Prometheus
```

URL:

```text
http://prometheus-server
```

Click:

```text
Save & Test
```

---

# Step 25: Import Dashboard in Grafana

Go to:

```text
Dashboards
→ Import
```

Dashboard ID:

```text
315
```

Select Prometheus datasource.

Click:

```text
Import
```

---

# Project Output

The project successfully demonstrated:

* Git version control operations
* Docker containerization
* Jenkins CI/CD automation
* Kubernetes deployment and orchestration
* Prometheus monitoring
* Grafana visualization dashboards
* AWS EC2 virtualization

---

# Advantages of the Project

* Automated deployment process
* Scalable Kubernetes deployment
* Real-time monitoring and visualization
* Improved application management
* Better reliability and availability
* Faster development lifecycle

---

# Challenges Faced

* Docker permission issues
* Jenkins Java compatibility issue
* Kubernetes image pull issue
* Port forwarding conflicts
* Minikube networking limitations on EC2

---

# Conclusion

This project successfully implemented a complete DevOps pipeline using modern DevOps tools and cloud infrastructure. The Node.js application was containerized using Docker, deployed through Jenkins CI/CD, orchestrated using Kubernetes, and monitored using Prometheus and Grafana. The project demonstrated automation, scalability, monitoring, and cloud deployment practices in a real-world DevOps environment.
