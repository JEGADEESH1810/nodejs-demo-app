# nodejs-demo-app — CI/CD Pipeline with GitHub Actions & Docker

This repository is my submission for **DevOps Internship – Task 1: Automate Code Deployment Using CI/CD Pipeline (GitHub Actions)**.

## 🎯 Objective

Set up a CI/CD pipeline that automatically builds, tests, and pushes a Docker image for a simple Node.js web application whenever code is pushed to the `main` branch.

## 🛠 Tools Used

* **GitHub** – Source control and repository hosting
* **GitHub Actions** – CI/CD automation
* **Node.js / Express** – Sample web application
* **Jest + Supertest** – Automated testing
* **Docker** – Containerization
* **Docker Hub** – Docker image registry

## 📁 Project Structure

```text
nodejs-demo-app/
├── .github/
│   └── workflows/
│       └── ci-cd.yml        # CI/CD pipeline definition
├── test/
│   └── app.test.js          # Automated tests
├── app.js                   # Express application
├── package.json
├── package-lock.json
├── Dockerfile
├── .dockerignore
├── .gitignore
└── README.md
```

## ⚙️ How the Pipeline Works

The pipeline is defined in `.github/workflows/ci-cd.yml`.

The workflow is triggered when code is pushed to the `main` branch.

The pipeline performs the following steps:

1. Checks out the source code.
2. Sets up Node.js.
3. Installs the application dependencies.
4. Runs automated tests.
5. Builds the Docker image.
6. Logs in to Docker Hub using GitHub Secrets.
7. Pushes the Docker image to Docker Hub.

The pipeline flow is:

```text
GitHub Push to main
        ↓
GitHub Actions
        ↓
Install Dependencies
        ↓
Run Automated Tests
        ↓
Build Docker Image
        ↓
Push Image to Docker Hub
        ↓
Pipeline Success
```

The Docker build and push job runs only after the test job completes successfully.

## 🚀 Run Locally

### 1. Clone the repository

```bash
git clone https://github.com/JEGADEESH1810/nodejs-demo-app.git
cd nodejs-demo-app
```

### 2. Install dependencies

```bash
npm install
```

### 3. Run tests

```bash
npm test
```

### 4. Start the application

```bash
npm start
```

The application will run on:

```text
http://localhost:3000
```

## 🐳 Run with Docker Locally

### Build the Docker image

```bash
docker build -t nodejs-demo-app .
```

### Run the Docker container

```bash
docker run -d -p 3000:3000 --name nodejs-demo-container nodejs-demo-app
```

### Check the running container

```bash
docker ps
```

### Test the application

Open the following URL in a browser:

```text
http://localhost:3000
```

You can also test using:

```bash
curl http://localhost:3000
```

and:

```bash
curl http://localhost:3000/health
```

### Stop the container

```bash
docker stop nodejs-demo-container
```

### Start the container again

```bash
docker start nodejs-demo-container
```

## 🔐 GitHub Secrets

Docker Hub credentials are stored securely using GitHub repository secrets.

Go to:

**GitHub Repository → Settings → Secrets and variables → Actions**

The following secrets are used:

| Secret Name          | Purpose                 |
| -------------------- | ----------------------- |
| `DOCKERHUB_USERNAME` | Docker Hub username     |
| `DOCKERHUB_TOKEN`    | Docker Hub Access Token |

The Docker Hub access token is used instead of storing a Docker Hub password in the workflow.

Sensitive credentials are not hard-coded in the repository.

## 🐳 Docker Hub

The Docker image is automatically pushed to Docker Hub after the tests and Docker build complete successfully.

**Docker Hub Repository:**

`jegadeesh1810/nodejs-demo-app`

**Docker Image:**

`jegadeesh1810/nodejs-demo-app:latest`

## ✅ Verifying the Pipeline

To verify the CI/CD pipeline:

1. Push a change to the `main` branch.
2. Open the **Actions** tab in the GitHub repository.
3. Check the workflow execution.
4. Verify that the test job completes successfully.
5. Verify that the Docker build and push job completes successfully.
6. Check Docker Hub to confirm that the Docker image has been pushed.

The successful pipeline flow is:

```text
Test ✅
   ↓
Docker Build ✅
   ↓
Docker Push ✅
```

## 📸 Screenshots

The following screenshots can be provided as evidence of the completed task:

* GitHub repository showing the project files
* `.github/workflows/ci-cd.yml` workflow
* GitHub Actions successful workflow run
* Successful automated test job
* Successful Docker build and push job
* Docker Hub repository showing the Docker image
* Node.js application running locally
* Docker container running locally

## 📌 Project Status

The CI/CD pipeline has been successfully implemented and tested.

* ✅ Node.js application
* ✅ Express web application
* ✅ Automated Jest tests
* ✅ Dockerfile
* ✅ Docker image build
* ✅ Docker container
* ✅ GitHub repository
* ✅ GitHub Actions workflow
* ✅ GitHub Secrets
* ✅ Docker Hub repository
* ✅ Docker image push
* ✅ Automated Test → Docker Build → Docker Push pipeline

## 📚 Interview Questions & Answers

### 1. What is CI/CD?

CI/CD stands for Continuous Integration and Continuous Delivery/Deployment.

**Continuous Integration (CI)** automatically builds and tests code whenever changes are pushed to the repository.

**Continuous Delivery/Deployment (CD)** automates the process of delivering or deploying the tested application.

In this project, GitHub Actions is used to automate testing, Docker image building, and pushing the image to Docker Hub.

### 2. How do GitHub Actions work?

GitHub Actions uses workflow files written in YAML and stored inside the `.github/workflows/` directory.

A workflow is triggered by events such as a push to the `main` branch. It contains jobs and steps that run on GitHub-hosted runners.

In this project, GitHub Actions automatically runs the tests and then builds and pushes the Docker image.

### 3. What are GitHub Actions runners?

Runners are the machines that execute GitHub Actions jobs.

GitHub provides hosted runners such as Ubuntu, Windows, and macOS environments.

This project uses a GitHub-hosted runner to execute the CI/CD workflow.

### 4. What is the difference between jobs and steps?

A **job** is a group of steps that runs on a runner.

A **step** is an individual task inside a job, such as checking out code, installing dependencies, running tests, or building a Docker image.

Jobs can run independently or depend on another job using `needs:`.

### 5. How are secrets secured in GitHub Actions?

Sensitive information such as Docker Hub credentials should not be written directly inside the workflow file.

GitHub Secrets are used to securely store values such as:

```text
DOCKERHUB_USERNAME
DOCKERHUB_TOKEN
```

The workflow accesses these values securely through GitHub Secrets.

### 6. How does the pipeline handle deployment errors?

The Docker deployment job depends on the successful completion of the test job.

If the tests fail, the Docker image is not pushed.

This helps prevent broken code from being deployed to Docker Hub.

### 7. Explain the Docker build and push process.

First, Docker reads the `Dockerfile` and builds an image:

```bash
docker build -t nodejs-demo-app .
```

The GitHub Actions workflow then authenticates with Docker Hub using the configured secrets.

Finally, the Docker image is pushed to Docker Hub:

```text
jegadeesh1810/nodejs-demo-app:latest
```

The image can then be pulled and run on another system.

### 8. How can the CI/CD pipeline be tested locally?

The individual parts of the pipeline can be tested locally before pushing code to GitHub.

For example:

```bash
npm test
```

can be used to test the Node.js application.

The Docker image can also be tested locally using:

```bash
docker build -t nodejs-demo-app .
docker run -d -p 3000:3000 --name nodejs-demo-container nodejs-demo-app
```

After confirming that everything works locally, the changes can be pushed to GitHub to trigger the automated GitHub Actions pipeline.

## 🔗 Repository

**GitHub Repository:**

https://github.com/JEGADEESH1810/nodejs-demo-app

**Docker Hub Repository:**

https://hub.docker.com/r/jegadeesh1810/nodejs-demo-app

---

## 🎉 Conclusion

This project successfully demonstrates a basic CI/CD pipeline using **GitHub Actions, Node.js, Jest, Docker, and Docker Hub**.

The completed workflow automatically performs:

**Code Push → Test → Docker Build → Docker Push**

The pipeline has been successfully executed and verified.

---

*Submitted as part of the Elevate Labs DevOps Internship.*
