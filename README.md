# Jenkins CI/CD Pipeline Project (Beginner-Friendly DevOps)

This project demonstrates how to set up a basic CI/CD pipeline using **Jenkins**, **Docker**, and **AWS EC2**, based on the YouTube tutorial [DevOps Project from Scratch](https://www.youtube.com/watch?v=jv47sPxhUf8).

## 🔧 Tools Used
- **AWS EC2** (2 instances)
- **Jenkins**
- **Docker**
- **Docker Compose**
- **Tomcat (Docker image)**
- **Jenkins plugins**: Publish Over SSH

---

## 🖥️ Architecture Overview

- **EC2 Instance 1**: Jenkins Server
- **EC2 Instance 2**: Docker host running Tomcat container
- SSH Key pair is shared between both instances for secure communication.

---

## 🚀 Setup Guide

### 1. Launch EC2 Instances
- Launch **two Ubuntu EC2 instances** in the same region.
- Assign the same key pair to both.
- Open the following ports in the **security groups**:
  - Port 8080 (Jenkins)
  - Port 22 (SSH)
  - Port 80/443 (if deploying a web app)

### 2. Set Up Jenkins (on EC2 Instance 1)

```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

### 3. Install Jenkins Plugins
- Go to Manage Jenkins > Plugins.
- Install:
  - Publish Over SSH
  - Git, GitHub plugins)
 
### 4. Set Up Docker Host (on EC2 Instance 2)
 ```bash
sudo apt update && sudo apt-get update
sudo apt install docker.io
sudo usermod -aG docker ubuntu
sudo hostname Docker-Host
sudo nano /etc/hostname
sudo init 6
clear
sudo chown -R ubunru:ubuntu /opt
cd /opt
mkdir Docker
sudo chown -R ubuntu:ubuntu /opt/Docker
cd Dokcer/
nano Dockerfile
docker build -t webapp:v1 .
docker run -d --name registerapp -p 8086:8080 webapp:v1
```

Test with 'docker --version' and 'docker-compose --version'

📌 Notes

Ensure both instances are in the same VPC/subnet for low-latency SSH.
You can add a sample WAR file or a simple Java app to deploy via Tomcat.
📚 Reference

👨🏾‍💻 Author

Kelechi Anyanwu
DevOps Enthusiast | Computer Science Student
📫 Connect with me on LinkedIn
