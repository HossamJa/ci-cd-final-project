# CI/CD Final Project - Counter Microservice with GitHub Actions and OpenShift Pipelines

This repository contains the source code and pipeline configurations for the final project of the IBM course **"Continuous Integration and Continuous Delivery (CI/CD)"**. The project demonstrates the implementation of a complete CI/CD pipeline for a Python-based microservice using GitHub Actions and OpenShift Pipelines with Tekton.

---

## 🧩 Project Overview

You're part of a DevOps team responsible for the backend of a **counter microservice** API. The frontend is already built. Your job is to ensure continuous integration and deployment of this microservice using modern DevOps practices.

---

## ✅ Objectives

- Create a CI pipeline in GitHub Actions to:
  - Lint the code using `flake8`
  - Run unit tests using `nose`
- Create CD pipeline using OpenShift Pipelines with Tekton:
  - Tasks for cleanup, linting, testing, building image, and deployment
- Deploy the application on an OpenShift cluster using Tekton and `oc` commands

---

## 📁 Project Structure

```bash
.
├── .github/workflows/
│   ├── workflow.yml        # GitHub Actions CI workflow (lint + test)
│
├── .tekton/
│   ├── tasks.yml           # Tekton tasks for cleanup, nose tests, build, deploy
│
├── bin/
│   └── setup.sh            # Project setup script
│
├── service/
│   ├── common/
│   │   ├── error_handlers.py
│   │   ├── log_handlers.py
│   │   └── status.py
│   ├── routes.py
│   └── tests/
│       └── test_routes.py  # Nose unit tests
│
├── Dockerfile
├── requirements.txt
├── setup.cfg
├── LICENSE
├── Procfile
└── README.md               # ← You are here!
```

---

## 🔧 CI Pipeline (GitHub Actions)

Located at: [.github/workflows/workflow.yml](.github/workflows/workflow.yml)

### Trigger
- On `push` and `pull_request` to the `main` branch

### Steps
- Checkout the code
- Install dependencies from `requirements.txt`
- Run **flake8** linting:
  ```bash
  flake8 service --count --select=E9,F63,F7,F82 --show-source --statistics
  flake8 service --count --max-complexity=10 --max-line-length=127 --statistics
  ```
- Run **nose** tests:
  ```bash
  nosetests -v --with-spec --spec-color --with-coverage --cover-package=app
  ```

📸 *Screenshot of successful GitHub Actions CI pipeline*  
![GitHub Actions CI Validation](./screenshots/cicd-github-validate.png)

---

## 🚀 CD Pipeline (OpenShift Pipelines with Tekton)

Located at: [.tekton/tasks.yml](.tekton/tasks.yml)

### Tekton Tasks
- **cleanup**: Clears output workspace before build
- **nose**: Runs unit tests
- **Additional Tasks**: clone, lint, buildah image build, deploy

### PVC
- Created using OpenShift console
- Name: `oc-lab-pvc`
- StorageClass: `skills-network-learner`
- Size: 1GB

📸 *PVC details in OpenShift*  
![PVC Details](./screenshots/oc-pipelines-console-pvc-details.png)

### OpenShift Pipeline Steps
1. Cleanup
2. Git Clone
3. Lint
4. Test
5. Build Image
6. Deploy with `oc`

📸 *Pipeline setup and execution*  
- Pipeline YAML: ![Pipeline YAML](./screenshots/oc-pipelines-oc-final.png)  
- Successful run: ![Pipeline Success](./screenshots/oc-pipelines-oc-green.png)  
- Application Logs: ![App Logs](./screenshots/oc-pipelines-app-logs.png)

---

## 📦 Deployment

- Image is built using `buildah`
- Application deployed to OpenShift with:
  ```bash
  oc create deployment $(params.app-name) --image=$(params.build-image) --dry-run=client -o yaml | oc apply -f -
  ```

---

## 📸 Screenshots Checklist

| Description                              | Filename                                 |
|------------------------------------------|------------------------------------------|
| GitHub Actions successful run            | `cicd-github-validate.png`               |
| OpenShift PVC Details                    | `oc-pipelines-console-pvc-details.png`   |
| OpenShift Pipeline definition            | `oc-pipelines-oc-final.png`              |
| OpenShift Pipeline successful execution  | `oc-pipelines-oc-green.png`              |
| Application logs                         | `oc-pipelines-app-logs.png`              |

All screenshots are stored in the `screenshots/` folder.

---

## 📬 Submission Checklist

- [x] CI workflow in `.github/workflows/workflow.yml`
- [x] Tekton tasks in `.tekton/tasks.yml`
- [x] Working GitHub Actions CI
- [x] Working OpenShift CD pipeline
- [x] Screenshots added for all required steps

---

## 📎 License

This project is licensed under the [MIT License](./LICENSE).

---

## 🙌 Acknowledgment

Built as part of the **IBM DevOps and Software Engineering** certification track.
