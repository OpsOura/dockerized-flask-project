
#  DevOps Capstone Project: Dockerized Flask + MySQL App with CI/CD and Minikube

## Overview
This project is a beginner-friendly **DevOps Capstone** that demonstrates a complete CI/CD pipeline using:

- **Flask** as a web application  
- **MySQL** as the backend database  
- **Docker** and **Docker Compose** for containerization  
- **GitHub Actions** for CI/CD automation  
- **Minikube** to simulate container orchestration  
- *(Optional)* **Prometheus** and **Grafana** for basic monitoring  

By the end, you'll have a working DevOps pipeline from **code → container → CI/CD → local deployment**.

---

##  Prerequisites

Make sure you have these installed:
- [Docker & Docker Compose](https://docs.docker.com/get-docker/)
- [Git](https://git-scm.com/downloads)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- GitHub account (for Actions CI/CD)

---

##  Step 1: Clone the Repository
```bash
git clone https://github.com/OpsOura/dockerized-flask-project.git
cd dockerized-flask-project
````

---

##  Step 2: Create Environment File

Create a `.env` file in the project root with your MySQL settings:

```bash
MYSQL_HOST=mysql
MYSQL_USER=root
MYSQL_PASSWORD=yourpassword
MYSQL_DB=messagesdb
```

---

##  Step 3: Build and Run with Docker Compose

Use Docker Compose to build and run both containers (Flask + MySQL):

```bash
docker-compose up --build
```

Once started, open your browser and visit:

```
http://localhost
```

If something goes wrong, check logs:

```bash
docker-compose logs -f
```

---

##  Step 4: Create MySQL Table

Connect to your MySQL container:

```bash
docker exec -it <mysql_container_name> mysql -u root -p
```

Then create the table:

```sql
CREATE TABLE messages (
    id INT AUTO_INCREMENT PRIMARY KEY,
    message TEXT
);
```

---

## ⚡ Step 5: Set Up CI/CD with GitHub Actions

Create a new folder and workflow file:

```
.github/workflows/ci.yml
```

Paste this inside:

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Build Docker image
        run: docker build -t flaskapp .

      - name: Run Docker Compose (test)
        run: docker-compose up -d
```

Commit and push the workflow file:

```bash
git add .
git commit -m "Add GitHub Actions CI/CD"
git push origin main
```

Now go to the **Actions** tab in your GitHub repo — the pipeline will run automatically on every push to the `main` branch.

---

##  Step 6: Deploy on Minikube

We’ll use **Minikube** just to simulate a Kubernetes-like environment (no complex YAML files needed).

Start Minikube:

```bash
minikube start
```

Use Minikube’s Docker environment:

```bash
eval $(minikube docker-env)
```

Build your Docker image inside Minikube:

```bash
docker build -t flaskapp .
```

Run the app as a pod:

```bash
kubectl run flask-app --image=flaskapp --port=5000
```

Expose it as a service:

```bash
kubectl expose pod flask-app --type=NodePort --port=5000
```

Access the app:

```bash
minikube service flask-app
```

---

##  Step 7: (Optional) Add Monitoring

You can optionally integrate **Prometheus** and **Grafana** using simple Docker containers.

Example (optional):

```bash
docker run -d -p 9090:9090 prom/prometheus
docker run -d -p 3000:3000 grafana/grafana
```

Then open:

* Prometheus → [http://localhost:9090](http://localhost:9090)
* Grafana → [http://localhost:3000](http://localhost:3000)

---

##  Troubleshooting Guide

| Issue                        | Possible Fix                                             |
| ---------------------------- | -------------------------------------------------------- |
| App not loading              | Check `docker ps` and ensure both containers are running |
| MySQL connection error       | Verify `.env` variables and container network            |
| CI/CD pipeline fails         | Check GitHub Actions logs under the "Actions" tab        |
| Minikube service not opening | Run `minikube service list` and verify the port          |

---

##  Outcome

By completing this project, you’ll gain hands-on experience with:

* Containerizing applications using Docker
* Running multi-container apps using Docker Compose
* Building CI/CD pipelines with GitHub Actions
* Deploying containers on Minikube
* (Optionally) Setting up monitoring tools

You’ve just built your own **mini DevOps pipeline**, similar to what real companies use!

---

##  Credits

This project is part of the **DevOps Capstone Series** — a simplified, beginner-friendly approach to real-world DevOps tools and workflows.

```

```
