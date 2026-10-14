# FA1 – DevOps Environment Manager

**Student:** Vijay Dhame

## Overview
This repository contains the complete implementation of the FA1 practical exam:

- **IaC with Terraform** – Docker provider, workspaces for QA, UAT, PROD, isolated state.
- **Docker containers** – Backend (FastAPI) and Frontend (React‑Vite) images built locally.
- **Multi‑environment deployment** – Each environment runs on its own Docker network and uses unique host ports (8081/8001, 8082/8002, 8083/8003).
- **Verification scripts** – Health endpoints, automated pytest suite.
- **Documentation** – README (this file), PowerPoint presentation, deployment scripts.

## Project Structure
```
fa1-devops-environment-manager/
│   README.md               # ← this file
│   FA1_DevOps_Environment_Manager.pptx   # Presentation
│   docker-compose.yml
│   .gitignore
│   .dockerignore
│
├─ backend/
│   ├─ app/
│   │   ├─ __init__.py
│   │   └─ main.py
│   ├─ requirements.txt
│   ├─ Dockerfile
│   └─ tests/
│
├─ frontend/
│   ├─ src/
│   ├─ package.json
│   ├─ vite.config.ts
│   └─ Dockerfile
│
└─ terraform/
    ├─ main.tf
    ├─ variables.tf
    ├─ versions.tf
    ├─ outputs.tf
    └─ environments/
        ├─ qa.tfvars
        ├─ uat.tfvars
        └─ prod.tfvars
```

## Quick Start
1. **Install dependencies**
   ```powershell
   # Backend (Python)
   python -m venv venv
   .\venv\Scripts\activate
   pip install -r backend/requirements.txt

   # Frontend (Node)
   cd frontend
   npm ci
   cd ..
   ```

2. **Build Docker images**
   ```powershell
   docker build -t devops-env-manager-backend:latest ./backend
   docker build -t devops-env-manager-frontend:latest ./frontend
   ```

3. **Initialize Terraform**
   ```powershell
   cd terraform
   terraform init
   ```

4. **Deploy an environment** (example QA)
   ```powershell
   terraform workspace new qa   # or select if already exists
   terraform workspace select qa
   terraform apply -var-file=environments/qa.tfvars -auto-approve
   ```
   Repeat for `uat` and `prod`.

5. **Verify**
   - Backend health: `http://localhost:<backend_port>/health`
   - Frontend UI: `http://localhost:<frontend_port>`
   - Dashboard shows the status of all three environments.

6. **Destroy**
   ```powershell
   terraform destroy -var-file=environments/qa.tfvars -auto-approve
   # repeat for other workspaces
   ```

## Presentation
The PowerPoint `FA1_DevOps_Environment_Manager.pptx` summarises the architecture, workflow, verification, and viva preparation checklist.

---
*Prepared for the FA1 practical examination.*
