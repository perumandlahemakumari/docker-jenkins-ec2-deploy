# 🐳 Dockerized Web Application with Jenkins CI/CD Pipeline

## 🚀 Project Overview

This project demonstrates how to:
- Set up a **Dockerized web application** hosted on **EC2**
- Build and deploy it using a **Jenkins Pipeline**
- Open the app in a browser via **Docker container on port 8089**

GitHub Repo Used:  
🔗 `https://github.com/suneelprojects/docker-site.git`

---

## 🖥️ EC2 Setup Instructions

### Step 1: Launch an Amazon Linux EC2 Instance
- Choose Amazon Linux 2
- Allow inbound port **22 (SSH)** and **8089 (HTTP)** in the Security Group

### Step 2: Install Docker & Jenkins

```bash
sudo yum update -y
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo yum install java-17-amazon-corretto -y
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum install jenkins -y
sudo systemctl start jenkins
```

### Step 3: Allow Jenkins to Use Docker

```bash
sudo usermod -aG docker jenkins
sudo chmod 777 /var/run/docker.sock
```

> Restart EC2 or Jenkins service if required.

---

## 🛠️ Jenkins Setup (Freestyle or Pipeline)

### Step 4: Create a Pipeline Job in Jenkins

Use the following **Jenkinsfile**:

```groovy
pipeline {
    agent any
    stages {
        stage('code') {
            steps {
                git 'https://github.com/suneelprojects/docker-site.git'
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t hema:v1 html'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker run -itd --name cycle -p 8089:80 hema:v1'
            }
        }
    }
}
```

---

## 🌐 Access the Application

After the pipeline completes:

- Visit the app in your browser:  
  `http://<EC2-Public-IP>:8089`

---

## 📌 Notes

- If the container is already running, stop and remove it before rerunning the pipeline:
  ```bash
  docker rm -f cycle
  ```

- To check if it's running:
  ```bash
  docker ps
  ```

- To view logs:
  ```bash
  docker logs cycle
  ```

---

## ✅ Summary

- Jenkins automates the pull → build → deploy process
- Docker hosts the web app on port 8089
- EC2 serves as a minimal production-like environment

---
