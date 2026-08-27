# nodejs-demo-app — CI/CD Pipeline with GitHub Actions & Docker

This repository is my submission for **DevOps Internship – Task 1: Automate Code Deployment Using CI/CD Pipeline (GitHub Actions)**.

## 🎯 Objective
Set up a CI/CD pipeline that automatically builds, tests, and deploys (pushes a Docker image for) a simple Node.js web app whenever code is pushed to the `main` branch.

## 🛠 Tools Used
- **GitHub** – source control & repo hosting
- **GitHub Actions** – CI/CD automation
- **Node.js / Express** – sample web app
- **Jest + Supertest** – automated tests
- **Docker** – containerization
- **Docker Hub** – image registry for deployment

## 📁 Project Structure
```
nodejs-demo-app/
├── .github/
│   └── workflows/
│       └── main.yml        # CI/CD pipeline definition
├── test/
│   └── app.test.js         # automated tests
├── app.js                  # Express app
├── package.json
├── Dockerfile
├── .dockerignore
├── .gitignore
└── README.md
```

## ⚙️ How the Pipeline Works
The pipeline is defined in `.github/workflows/main.yml` and has two jobs:

1. **build** (runs on every push/PR to `main`)
   - Checks out the code
   - Sets up Node.js 18
   - Installs dependencies (`npm install`)
   - Runs automated tests (`npm test`)

2. **deploy** (runs only after `build` succeeds, and only on a push to `main`)
   - Logs in to Docker Hub using repo secrets
   - Builds a Docker image from the `Dockerfile`
   - Pushes the image to Docker Hub

This gives the flow: **test → build → push**, exactly as required.

## 🚀 Run Locally

```bash
# 1. Clone the repo
git clone https://github.com/<your-username>/nodejs-demo-app.git
cd nodejs-demo-app

# 2. Install dependencies
npm install

# 3. Run tests
npm test

# 4. Start the app
npm start
# App will be running at http://localhost:3000
```

## 🐳 Run with Docker Locally

```bash
# Build the image
docker build -t nodejs-demo-app .

# Run the container
docker run -p 3000:3000 nodejs-demo-app

# Test it
curl http://localhost:3000
curl http://localhost:3000/health
```

## 🔐 Setting Up GitHub Secrets (required for the deploy job)
Go to your GitHub repo → **Settings → Secrets and variables → Actions → New repository secret**, and add:

| Secret name           | Value                                   |
|------------------------|------------------------------------------|
| `DOCKERHUB_USERNAME`   | Your Docker Hub username                |
| `DOCKERHUB_TOKEN`      | A Docker Hub Access Token (not your password) |

To generate a Docker Hub token: Docker Hub → Account Settings → Security → **New Access Token**.

## ✅ Verifying the Pipeline
1. Push any change to the `main` branch.
2. Go to the **Actions** tab in your GitHub repo.
3. You'll see the workflow run automatically — first the `build` job, then `deploy`.
4. Once green ✅, check Docker Hub — the image `<your-username>/nodejs-demo-app:latest` will be updated.

## 📸 Screenshots
_Add screenshots here after your first successful run, e.g.:_
- Screenshot of the green ✅ GitHub Actions run
- Screenshot of the image on Docker Hub

## 📚 Interview Questions & Answers

**1. What is CI/CD?**
CI/CD stands for Continuous Integration and Continuous Delivery/Deployment. CI is the practice of automatically building and testing code every time it's pushed, so integration issues are caught early. CD extends this by automatically delivering (or deploying) that tested code to a staging/production environment, reducing manual steps and speeding up releases.

**2. How do GitHub Actions work?**
GitHub Actions runs automated **workflows** defined in YAML files under `.github/workflows/`. A workflow is triggered by an **event** (like `push` or `pull_request`), and consists of one or more **jobs**, each made up of **steps** that run on a virtual machine called a **runner**. GitHub provisions the runner, executes the steps in order, and reports the result back to the repo.

**3. What are runners?**
Runners are the servers (virtual machines or containers) that actually execute the jobs in a workflow. GitHub provides free **hosted runners** (e.g., `ubuntu-latest`, `windows-latest`, `macos-latest`), or you can host your own **self-hosted runner** on your own infrastructure for more control or custom environments.

**4. Difference between jobs and steps.**
A **job** is a set of steps that run together on the same runner; jobs run in **parallel by default**, unless dependencies are set (e.g., using `needs:`). A **step** is a single task within a job — like running a command or using a pre-built action — and steps in a job run **sequentially**, one after another, sharing the same runner filesystem.

**5. How to secure secrets in GitHub Actions?**
Store sensitive values (API keys, passwords, tokens) as **GitHub Secrets** (repo/org level, under Settings → Secrets and variables → Actions) instead of hardcoding them. Reference them in workflows as `${{ secrets.SECRET_NAME }}` — GitHub automatically masks their values in logs. Never print secrets to console, avoid secrets in forked-PR workflows (they're restricted by default), and use environment-scoped secrets with required reviewers for production deployments.

**6. How to handle deployment errors?**
Use `if:` conditions so later jobs (like `deploy`) only run if earlier jobs (like `build`/`test`) succeed — this prevents deploying broken code. Add proper exit codes and error handling in scripts, use `continue-on-error` sparingly for non-critical steps, set up notifications (Slack/email) on failure, and always have a rollback plan (e.g., re-deploy the last known-good image tag).

**7. Explain the Docker build-push workflow.**
First, `docker build -t <image>:<tag> .` reads the `Dockerfile` and builds a local image layer by layer. Then `docker login` authenticates against the registry (e.g., Docker Hub) using credentials/tokens. Finally `docker push <image>:<tag>` uploads the built image to the registry, making it available for deployment on any server or orchestrator that pulls from that registry.

**8. How can you test a CI/CD pipeline locally?**
Tools like [`nektos/act`](https://github.com/nektos/act) let you run GitHub Actions workflows locally in Docker containers, simulating the actual runner environment without pushing to GitHub. You can also test individual pieces manually — run `npm test` locally, run `docker build` and `docker run` locally to confirm the image works — before ever pushing, which catches most issues early.

---
*Submitted as part of the Elevate Labs DevOps Internship.*
