# FastAPI Docker Deployment

This repository demonstrates how to containerize a FastAPI application using Docker and automate the deployment with GitHub Actions.

---

## 🛠 Installation & Running Locally

1. **Clone the repository:**
   ```sh
   git clone https://github.com/PrinceZuko10/fastapi-docker.git
   cd fastapi-docker
   ```
2. **Create a virtual environment and install dependencies:**
   ```sh
   python -m venv venv
   source venv/bin/activate  # On Windows use `venv\Scripts\activate`
   pip install -r requirements.txt
   ```
3. **Run the FastAPI server:**
   ```sh
   uvicorn main:app --reload
   ```
4. **Access the API:**
   - Open `http://127.0.0.1:8000`
   - Swagger Docs: `http://127.0.0.1:8000/docs`

---

## 🐳 Build and Run with Docker

1. **Build the Docker image:**
   ```sh
   docker build -t somit123/fastapi .
   ```
2. **Run the Docker container:**
   ```sh
   docker run -p 8000:8000 somit123/fastapi
   ```
3. **Access the API:**
   - Open `http://localhost:8000`

---

## ⚙️ GitHub Actions Workflow Explanation

The GitHub Actions workflow automates the Docker image build and push process:

- The workflow triggers on every `push` to the `main` branch.
- It logs into Docker Hub using secrets stored in GitHub.
- It builds the Docker image and pushes it to Docker Hub.

**Workflow File: `.github/workflows/DockerBuild.yml`**

```yaml
name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and push Docker image
        run: |
          docker build -t somit123/fastapi .
          docker push somit123/fastapi
```

---

## 🔑 Setting Up Docker Token and Secrets

1. **Create a Docker Hub account** (if not already created).
2. **Generate an access token:**
   - Go to [Docker Hub](https://hub.docker.com/settings/security)
   - Click `New Access Token`, give it a name, and copy it.
3. **Add GitHub Secrets:**
   - Go to your repository → `Settings` → `Secrets and variables` → `Actions`
   - Click `New repository secret`
   - Add the following secrets:
     - `DOCKER_USERNAME`: Your Docker Hub username.
     - `DOCKER_PASSWORD`: The access token from Docker Hub.

---

Now, every push to `main` will automatically build and push the Docker image to Docker Hub. 🚀
