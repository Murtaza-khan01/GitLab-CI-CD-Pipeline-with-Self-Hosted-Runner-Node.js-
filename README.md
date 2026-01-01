# 🚀 GitLab CI/CD Pipeline with Self-Hosted Runner (Node.js)

This repository demonstrates a **complete, production-style CI/CD pipeline** implemented using **GitLab CI/CD** with a **self-hosted GitLab Runner** for a containerized Node.js application.

The project covers everything from **runner setup** to **Docker image build & push**, **deployment**, and **real-world issues faced and resolved during pipeline execution**.

---

## 📌 Project Overview

The goal of this project was to:

* Automate application delivery using **GitLab CI/CD**
* Use a **self-hosted GitLab Runner** for better control and learning
* Containerize a Node.js application using **Docker**
* Push images securely to **DockerHub**
* Deploy the application and verify access via browser
* Understand CI/CD failures and debug them like a real DevOps workflow

---

## 🧱 CI/CD Pipeline Stages

The pipeline consists of the following stages:

```text
build  →  test  →  push_to_dockerhub  →  deploy
```

### 🔹 Build

* Builds the Node.js application
* Creates a Docker image locally on the runner

### 🔹 Test

* Executes basic validation/tests
* Ensures the application is ready for deployment

### 🔹 Push to DockerHub

* Tags Docker image properly
* Pushes the image securely to DockerHub

### 🔹 Deploy

* Pulls the latest image
* Runs the container
* Application becomes accessible via browser

---

## 🖥️ Self-Hosted GitLab Runner Setup

A **self-hosted GitLab Runner** was used instead of shared runners to:

* Gain full control over the execution environment
* Learn real-world CI infrastructure
* Avoid shared runner limitations
* Execute Docker commands directly

### Runner Installation (Linux)

```bash
sudo apt update
sudo apt install gitlab-runner -y
```

### Runner Registration

```bash
sudo gitlab-runner register
```

During registration:

* Executor: `shell` or `docker`
* Tags assigned for pipeline jobs
* Linked runner with the GitLab project

---

## 🐳 Docker Setup

### Docker Installed on Runner

```bash
sudo apt install docker.io -y
sudo usermod -aG docker gitlab-runner
```

Docker is required for:

* Building images
* Tagging images
* Pushing to DockerHub
* Running containers during deployment

---

## 🔐 GitLab CI/CD Variables

The following CI/CD variables were configured in **GitLab → Settings → CI/CD → Variables**:

| Variable Name    | Description                       |
| ---------------- | --------------------------------- |
| `DOCKERHUB_USER` | DockerHub username                |
| `DOCKERHUB_PASS` | DockerHub password / access token |

> ⚠️ Variables were carefully handled to avoid exposure and scope issues.

---

## 🔑 Secure Docker Login in CI

Since GitLab CI runs in a **non-interactive (non-TTY) environment**, Docker login was handled using:

```bash
echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER" --password-stdin
```

This avoids authentication failures during pipeline execution.

---

## ⚠️ Issues Faced & Resolutions

### ❌ Issue 1: `Cannot perform an interactive login from a non TTY device`

**Cause:**

* Using `docker login -p` inside GitLab CI

**Fix:**

* Switched to `--password-stdin` method

---

### ❌ Issue 2: `invalid reference format`

**Cause:**

* Docker image name contained empty or mismatched CI variables
* Inconsistent variable naming

**Fix:**

* Ensured consistent usage of CI variables
* Verified variables at runtime
* Followed Docker naming rules (lowercase, valid format)

---

### ❌ Issue 3: CI/CD Variables Not Working

**Cause:**

* Variables marked as **Protected**
* Pipeline running on non-protected branch

**Fix:**

* Ran pipeline on protected branch (`master`)
* Understood variable scope and execution context

---

## 📸 Screenshots

The repository includes screenshots demonstrating:

* Successful GitLab pipeline execution
* All pipeline stages passing
* Application running and accessible via browser

```md
![Pipeline Success](images/pipeline-success.png)
![App Running](images/app-running.png)
```

> Place your screenshots inside an `images/` directory.

---

## 🚀 Why GitLab CI/CD?

* Built-in CI/CD with Git repository
* Clean pipeline visualization
* Secure secret management
* Minimal configuration overhead
* Excellent support for self-hosted runners

---

## 🧠 Key Learnings

* CI/CD failures are usually **context-related**, not tool-related
* Variable scope and protection matter
* Self-hosted runners provide real production experience
* Debugging pipelines is a core DevOps skill

---

## 📎 Tech Stack

* GitLab CI/CD
* Self-hosted GitLab Runner
* Docker & DockerHub
* Node.js
* Linux

---

## ✅ Status

✔ Pipeline fully functional
✔ Docker images pushed successfully
✔ Application deployed and accessible

---

## 📬 Author

**Murtaza Khan**
DevOps Enthusiast | CI/CD | Docker | GitLab | Jenkins

---

⭐ If you find this project useful, feel free to star the repository!
