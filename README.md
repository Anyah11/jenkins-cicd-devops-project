# Jenkins CI/CD Pipeline Project (Beginner-Friendly DevOps)

This project demonstrates how to set up a basic CI/CD pipeline using **Jenkins**, **Docker**, and **AWS EC2**, based on the YouTube tutorial [DevOps Project from Scratch](https://www.youtube.com/watch?v=jv47sPxhUf8).

🚀 Live Demo: [Click here](http://54.160.184.96:8086/webapp/)  
⚠️ Note: This public IP may change.

## 🛠 Tools & Technologies Used

- **AWS EC2** – Hosted Jenkins and Docker on separate EC2 instances using the same key pair.
- **Jenkins** – Used to automate the CI/CD pipeline.
- **Docker & Docker Compose** – To containerize and deploy the web application.
- **Tomcat** – Used as the runtime server in the Docker container.
- **Publish Over SSH Plugin** – To enable Jenkins to deploy to the remote Docker EC2 instance.
- **Poll SCM** – Jenkins was configured to monitor the Git repository for changes using *Poll SCM* (cron-style schedule), ensuring automatic builds whenever code was pushed.



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

## 🔁 CI/CD Flow

1. Developer pushes code to GitHub.
2. Jenkins (configured with Poll SCM: `*/2 * * * *`) checks for changes every 2 minutes.
3. If changes are detected:
   - Jenkins pulls the latest code.
   - SSH connects to Docker EC2.
   - Docker Compose is triggered to build and restart the containers.
4. App is deployed to Tomcat inside a Docker container.

📌 Notes

Ensure both instances are in the same VPC/subnet for low-latency SSH.
You can add a sample WAR file or a simple Java app to deploy via Tomcat.
📚 Reference

👨🏾‍💻 Author

Kelechi Anyanwu
DevOps Enthusiast | Computer Science Student
📫 Connect with me on LinkedIn
