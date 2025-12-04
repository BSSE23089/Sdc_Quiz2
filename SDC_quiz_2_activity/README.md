#  Complete CI/CD, Docker, and Kubernetes Lab – Full Documentation

This repository contains the files for a **comprehensive lab exercise** demonstrating core concepts in:

- **Containerization (Docker)**
- **Multi-service orchestration (Docker Compose)**
- **Continuous Integration & Deployment (Jenkins)**
- **Container deployment on Kubernetes**

The project includes:
- A **Flask application** that connects to a **MySQL database**
- A complete **Jenkins CI/CD pipeline** for automated build, test, and Kubernetes deployment

---

##  Project Structure

| File | Description |
|------|-------------|
| **app.py** | Flask app that connects to a MySQL database. Includes retry logic. |
| **requirements.txt** | Python dependencies (flask, mysql-connector-python). |
| **docker-compose.yml** | Defines multi-service stack (Flask + MySQL) for local development. |
| **Dockerfile** | Builds the Nginx/HTML app for the CI/CD pipeline. |
| **index.html** | Static HTML page served by Nginx. |
| **Jenkinsfile** | Declarative Jenkins pipeline for build, test, and deploy. |
| **Dockerfile.jenkins** | Custom Jenkins controller image with Docker CLI installed. |
| **Dockerfile.agent** | Jenkins agent image including docker-ce-cli and kubectl. |
| **deployment.yaml** | Kubernetes Deployment manifest used in the CI/CD flow. |

---

#  Getting Started (Multi-Service Application)

The Flask + MySQL stack runs locally using Docker Compose.

##  **Prerequisites**
Make sure you have installed:

- Docker
- Docker Compose

---

##  1. Build and Run the Stack

Run the application using:

```bash
docker-compose up --build -d
```

This will:

- Build the Flask application image  
- Start a MySQL 8.0 container  
- Start the Flask web container  
- Create a persistent storage volume (`db_data`) for MySQL  

---

##  2. Access the Application

Open your browser and visit:

```
http://localhost:5001
```

If everything works correctly, you will see:

```
Hello from MySQL inside Docker!
```

---

##  3. Clean Up

To remove containers, networks, and volumes:

```bash
docker-compose down -v
```

---

#  CI/CD Pipeline – Jenkins + Kubernetes

The Jenkins pipeline builds, tests, and deploys the containerized application into a Kubernetes cluster.

The CI/CD system is powered by:

- **Dockerfile.jenkins** → Jenkins Controller with Docker CLI  
- **Dockerfile.agent** → Jenkins Agent with  
  - Docker  
  - kubectl  
  - Google Cloud SDK  

---

##  CI/CD Flow Breakdown (Jenkinsfile)

### **1️ Build Stage**
Builds a Docker image (`cicd-demo:v2`) for the Nginx web app:

```bash
docker build -t cicd-demo:v2 .
```

---

### **2️ Test Stage**
Runs a container test to verify the image:

```bash
docker run --rm cicd-demo:v2 echo "Container is working fine"
```

---

### **3️ Deploy Stage**
Deploys the application to Kubernetes:

```bash
kubectl apply -f deployment.yaml
```

The manifest deploys **2 replicas** of the `cicd-demo:v2` image.

---

#  Required CI/CD Setup

###  **1. Custom Jenkins Controller**
Built from `Dockerfile.jenkins`  
Includes `docker` CLI.

---

###  **2. Custom Jenkins Agent**
Agent built from `Dockerfile.agent`  
Includes:

- docker-ce-cli  
- kubectl  
- Google Cloud SDK (base image)

---

###  **3. Docker Socket Mounting**
Required for the Jenkins agent to run Docker commands:

```
/var/run/docker.sock
```

---

###  **4. Kubernetes Access**
The Jenkins agent must:

- Have `kubectl` installed  
- Have valid Kubernetes cluster access  
  (e.g., via kubeconfig or Google Cloud SDK auth)

---

#  Final Notes

This lab demonstrates the **complete lifecycle** of a modern containerized application:

1. Local development with Docker & Docker Compose  
2. Automated CI/CD using Jenkins  
3. Deployment to a Kubernetes cluster  

It’s a practical end-to-end workflow combining DevOps, CI/CD, containers, and Kubernetes orchestration.

---

If you want, I can **combine both README files**, generate **diagrams**, add **badges**, or create a **GitHub-ready professional README** with logos and visuals.