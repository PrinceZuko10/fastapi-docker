# FastAPI Server with Docker and GitHub Actions

##  How to Install and Run Locally

### **Prerequisites**
- Python 3.12+
- pip
- Virtual Environment (`venv`) (Optional)

  ### **Steps**
```sh
# Clone the repository
git clone https://github.com/your-username/fastapi-server.git
cd fastapi-server

# Create a virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run FastAPI server
uvicorn main:app --host 0.0.0.0 --port 8000
```

- Open **http://localhost:8000/docs** in the browser to test.

---

## 🐳 How to Build and Run Docker Image

### **Build the Docker Image**
```sh
docker build -t fastapi-app .
```

### **Run the Docker Container**
```sh
docker run -d -p 8000:8000 fastapi-app
```

- Open **http://localhost:8000/docs** in the browser.

### **Push to Docker Hub**
```sh
# Tag the image
docker tag fastapi-app your-dockerhub-username/fastapi-app:latest

# Login to Docker Hub
docker login

# Push the image
docker push your-dockerhub-username/fastapi-app:latest
```

---

## 🛠️ GitHub Actions Workflow Explanation
This repository includes a **GitHub Actions CI/CD pipeline** to build and push the Docker image automatically.

### **Workflow Steps**
1. **Triggers**: Runs on every push to `main` or when a pull request is opened.
2. **Builds the Docker Image**: Uses GitHub’s Ubuntu runner to build the FastAPI Docker image.
3. **Pushes to Docker Hub**: Logs into Docker Hub using secrets and pushes the built image.

### **GitHub Actions Workflow File (`.github/workflows/docker-publish.yml`)**
```yaml
name: Build and Push Docker Image

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2

      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and Push Docker image
        uses: docker/build-push-action@v4
        with:
          push: true
          tags: your-dockerhub-username/fastapi-app:latest
```

---

## 🔐 Steps for Setting Up Docker Token and Secrets

### **1. Generate a Docker Hub Access Token**
- Go to [Docker Hub](https://hub.docker.com/settings/security)
- Click **New Access Token**
- Name it **GitHub Actions** and copy the generated token.

### **2. Add Secrets to GitHub Repository**
- Go to **GitHub → Your Repository → Settings → Secrets and variables → Actions**
- Click **New Repository Secret** and add:
  - **`DOCKER_USERNAME`** = Your Docker Hub username
  - **`DOCKER_PASSWORD`** = The token you copied from Docker Hub

Now, GitHub Actions will automatically push the image to Docker Hub when you push code to `main`.

---

## 🎯 Next Steps
- Add Kubernetes deployment files.
- Set up automatic deployments with AWS/GCP.
- Implement CI/CD pipeline with GitHub Actions + Kubernetes.

---
