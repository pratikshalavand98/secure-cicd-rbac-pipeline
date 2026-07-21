# 🔐 Secure CI/CD Pipeline with Role-Based Access and Environment Promotion Strategy

## 📌 Project Overview

This project implements a secure and automated CI/CD pipeline using **Jenkins, GitHub, and AWS EC2**.

The objective is to address the security risks of unrestricted production deployments by introducing:

- Role-Based Access Control (RBAC)
- Automated multi-stage deployments
- Environment promotion strategy
- Manual approval before production deployment
- Separate EC2 instances for Dev, Staging, and Production
- Secure SSH-based application deployment

The deployment flow follows:

**Dev → Staging → Manual Approval → Production**

---

## 🎯 Scenario

In the existing environment:

- All developers could deploy directly to Production.
- No production approval workflow existed.
- No environment promotion strategy was implemented.
- There was no proper separation between Development, Staging, and Production environments.

Management required a secure CI/CD solution with proper access control and controlled environment promotion.

This project implements a Jenkins-based CI/CD pipeline that ensures applications are tested and promoted through environments before reaching Production.

---

## 🎯 Objectives

The main objectives of this project are:

1. Implement a secure CI/CD pipeline using Jenkins.
2. Store application source code in GitHub.
3. Automate application build and unit testing.
4. Deploy applications sequentially to Dev and Staging.
5. Introduce a manual approval gate before Production.
6. Implement Jenkins Role-Based Access Control.
7. Prevent Developers from directly approving Production deployments.
8. Allow Admin users to approve Production deployments.
9. Isolate Dev, Staging, and Production environments using separate EC2 instances.
10. Improve security, traceability, and deployment reliability.

---

## 🏗️ Architecture

![Secure CI/CD Pipeline Architecture](screenshots/secure-cicd-pipeline-architecture.png)

### Architecture Flow

```text
Developer
    |
    | Push Code
    v
GitHub Repository
    |
    v
Jenkins CI/CD Pipeline
    |
    v
Build
    |
    v
Unit Test
    |
    v
Deploy to Dev
    |
    v
Dev Environment
AWS EC2
    |
    v
Deploy to Staging
    |
    v
Staging Environment
AWS EC2
    |
    v
Manual Approval
Admin
    |
    v
Deploy to Production
    |
    v
Production Environment
AWS EC2
```
---

##  🛠️ Technologies & Tools

| Technology     | Purpose                                                    |
| -------------- | ---------------------------------------------------------- |
| Jenkins        | CI/CD automation and pipeline orchestration                |
| GitHub         | Source code management and version control                 |
| AWS EC2        | Hosting separate Dev, Staging, and Production environments |
| AWS IAM        | Identity and access management                             |
| Linux / Ubuntu | Server operating system                                    |
| Nginx          | Web server for hosting the application                     |
| SSH            | Secure remote server access                                |
| SCP            | Secure application file transfer                           |
| Groovy         | Jenkins Pipeline as Code                                   |

---
## 🚀 Application Setup
Sample Web Application

A simple HTML web application was created for deployment testing.

Application structure:

app/
└── index.html

The application displays:

Secure CI/CD Pipeline
Application Deployed Successfully
Environment Deployment by Jenkins

The application is deployed automatically by Jenkins to the Dev, Staging, and Production environments.

---

## 🛠️ Jenkins Installation and Server Setup

The Jenkins CI/CD server was configured on an Ubuntu EC2 instance using Java 21 and the official Jenkins Debian package repository.

1. Verify Java Installation

java --version

2. Update Ubuntu Package Repository

sudo apt update

3. Install Java 21

sudo apt install openjdk-21-jre-headless -y

4. Verify Java Version

java --version

5. Add the Jenkins Repository Signing Key

sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key

6. Add the Official Jenkins Repository

echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
https://pkg.jenkins.io/debian-stable binary/ | \
sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

7. Update Package Repository

sudo apt update

8. Install Jenkins

sudo apt install jenkins -y

9. Start Jenkins Service

sudo systemctl start jenkins

10. Enable Jenkins at System Boot

sudo systemctl enable jenkins

11. Check Jenkins Service Status

sudo systemctl status jenkins

Expected status:

Active: active (running)

12. Check Jenkins Service Configuration

sudo systemctl is-enabled jenkins

Expected output:

enabled

13. Check Jenkins Port

Jenkins runs on the default port 8080.

sudo ss -tulnp | grep 8080

14. Retrieve the Initial Jenkins Administrator Password

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

The generated password is used to complete the initial Jenkins setup through the web browser.

---

## 🧹 Jenkins Workspace and Server Cleanup

The following commands can be used to clean unnecessary files and free disk space when the Jenkins server has low available storage.

1. Check Current Disk Usage

df -h

2. Check Jenkins Workspace Usage

sudo du -sh /var/lib/jenkins/workspace

3. Stop Jenkins Before Cleaning Workspace

sudo systemctl stop jenkins

4. Remove Old Jenkins Workspace Files

sudo rm -rf /var/lib/jenkins/workspace/*

5. Clean Temporary Files

sudo rm -rf /tmp/*

6. Clean APT Package Cache

sudo apt clean

7. Remove Unused Packages

sudo apt autoremove -y

8. Reduce System Journal Logs

sudo journalctl --vacuum-size=200M

9. Start Jenkins Again

sudo systemctl start jenkins

10. Verify Jenkins Status

sudo systemctl status jenkins

11. Verify Available Disk Space

df -h

12. Verify Jenkins Workspace Usage

sudo du -sh /var/lib/jenkins/workspace

13. Verify Jenkins Node Status

After restarting Jenkins, verify the Built-In Node from:

Jenkins Dashboard
→ Manage Jenkins
→ Nodes
→ Built-In Node

The node should show:

Online

### Important Cleanup Note

The following command removes files from the Jenkins workspace:

sudo rm -rf /var/lib/jenkins/workspace/*

This removes workspace copies only. It does not remove the Jenkins job configuration or the GitHub repository.

Do not manually delete:

/var/lib/jenkins/jobs/

unless you intentionally want to remove Jenkins job configuration and build history.

For normal disk-space cleanup, cleaning the workspace, APT cache, temporary files, and old system logs is sufficient.

---

## 📂 Project Structure
```bash
secure-cicd-rbac-pipeline/
│
├── app/
│   └── index.html
│
├── screenshots/
│   ├── secure-cicd-pipeline-architecture.png
│   ├── jenkins-installation-bash-history.png
│   ├── aws-ec2-instances-overview.png
│   ├── jenkins-global-security-config.png
│   ├── jenkins-users-list.png
│   ├── jenkins-role-assignments.png
│   ├── jenkins-pipeline-creation.png
│   ├── jenkins-pipeline-dashboard.png
│   ├── jenkins-console-scm-checkout.png
│   ├── jenkins-manual-approval-gate.png
│   ├── jenkins-console-output-success.png
│   ├── jenkins-pipeline-stage-graph.png
│   ├── app-deployment-dev-environment.png
│   ├── app-deployment-staging-environment.png
│   └── app-deployment-prod-environment.png
│
├── Jenkinsfile
└── README.md
```

---
## 🔄 CI/CD Pipeline Design
The Jenkins pipeline consists of the following stages:
```bash
1. Checkout Source Code
          ↓
2. Environment Check
          ↓
3. Build
          ↓
4. Unit Test
          ↓
5. Deploy to Dev
          ↓
6. Deploy to Staging
          ↓
7. Manual Approval
          ↓
8. Deploy to Production
```
---

# 🔐 Secure CI/CD Pipeline with Role-Based Access Control

## 1. Checkout Source Code

Jenkins retrieves the application source code from the GitHub repository.

```text
GitHub Repository
        ↓
Jenkins Checkout SCM
```

This ensures that Jenkins works with the latest committed source code.

## 2. Environment Check

The pipeline verifies the Jenkins execution environment using commands such as:

```bash
whoami
hostname
date
```

This helps confirm the Jenkins execution user, server hostname, and current execution time.

## 3. Build

The Build stage verifies the application structure and required files.

**Example:**

```text
Building Application...
```

The application files are checked before deployment.

## 4. Unit Test

The pipeline executes the Unit Test stage before deployment.

```text
Running Unit Tests...
```

This ensures that the application passes validation before being promoted to the deployment environments.

## 5. Deploy to Dev

The application is deployed automatically to the Development EC2 server.

**Dev Server:** `44.201.216.9`

The deployment process uses secure SSH authentication and SCP file transfer.

### Application Deployment Flow

```text
Jenkins
    |
    | SCP
    v
Dev EC2
    |
    | Copy Application
    v
/var/www/html/index.html
    |
    v
Restart Nginx
```

## 6. Deploy to Staging

After successful Dev deployment, Jenkins automatically deploys the application to the Staging environment.

**Staging Server:** `44.211.168.120`

The Staging environment provides a separate environment for validating the application before Production deployment.

## 7. Manual Approval

Production deployment is protected by a manual approval gate.

The pipeline pauses and displays:

```text
Approve Production Deployment?
Deploy or Abort
```

Production deployment cannot continue until an authorized Admin approves the deployment.

**Example:**

```text
Approved by Admin
```

This prevents uncontrolled or accidental Production deployments.

## 8. Deploy to Production

After successful Admin approval, Jenkins deploys the application to the Production EC2 server.

**Production Server:** `13.222.41.40`

The Production deployment is executed only after:

```text
Build
  ↓
Unit Test
  ↓
Dev Deployment
  ↓
Staging Deployment
  ↓
Admin Approval
  ↓
Production Deployment
```

# 🔐 Role-Based Access Control

Jenkins Role-Based Access Control is implemented to restrict access to sensitive deployment operations.

## Developer Role

Developers are allowed to:

* Access Jenkins jobs according to assigned permissions.
* Trigger authorized builds.
* Participate in the development workflow.
* Deploy through the controlled Dev and Staging pipeline flow.

Developers are not allowed to directly approve Production deployment.

## Admin Role

Admins are allowed to:

* Manage Jenkins jobs.
* Manage deployment workflows.
* Approve Production deployments.
* Control Production release decisions.

Only authorized Admin users can approve the Production deployment gate.

### RBAC Workflow

```text
Developer
    |
    | Push Code
    v
GitHub
    |
    v
Jenkins
    |
    v
Dev Deployment
    |
    v
Staging Deployment
    |
    v
Production Approval
    |
    | Admin Approval Required
    v
Production Deployment
```

This ensures that Production deployment is controlled and not directly accessible to Developers.

# 🌐 Environment Isolation

Separate AWS EC2 instances are used for each deployment environment.

| Environment | Server IP        | Purpose                            |
| ----------- | ---------------- | ---------------------------------- |
| Dev         | `44.201.216.9`   | Development and initial deployment |
| Staging     | `44.211.168.120` | Pre-production validation          |
| Production  | `13.222.41.40`   | Final production deployment        |

Each environment is independently hosted on a separate EC2 instance.

This provides:

* Environment separation
* Reduced deployment risk
* Independent testing
* Production isolation
* Controlled application promotion

# 📈 Environment Promotion Strategy

The application follows a controlled promotion process:

```text
Developer Commit
       ↓
GitHub
       ↓
Jenkins
       ↓
Build
       ↓
Unit Test
       ↓
Deploy to Dev
       ↓
Deploy to Staging
       ↓
Manual Admin Approval
       ↓
Deploy to Production
```

Production is not deployed directly from the Developer environment.

The application must successfully pass through the Dev and Staging environments before Production deployment.

# 🛡️ Security Improvements

The implementation provides the following security improvements:

## 1. Role-Based Access Control

Jenkins RBAC limits access to deployment and approval operations based on user roles.

## 2. Production Access Restriction

Developers cannot directly approve Production deployments.

## 3. Manual Production Approval

Production deployment requires explicit approval from an authorized Admin.

## 4. Environment Isolation

Dev, Staging, and Production are hosted on separate EC2 instances.

## 5. Controlled Environment Promotion

Applications follow a defined promotion path:

```text
Dev → Staging → Production
```

## 6. Secure SSH Deployment

Application files are transferred using SCP and deployed using SSH-based access.

## 7. Automated CI/CD

Jenkins automates the deployment process and reduces manual deployment errors.

## 8. Deployment Traceability

Jenkins maintains pipeline execution history and approval information for deployment tracking.

# 📊 Pipeline Execution Result

The final pipeline execution successfully completed all required stages:

```text
Checkout SCM             ✅
Environment Check        ✅
Build                    ✅
Unit Test                ✅
Deploy to Dev            ✅
Deploy to Staging        ✅
Manual Approval          ✅
Deploy to Production     ✅
Pipeline Status          SUCCESS
```

The application was successfully deployed and verified in all three environments.

# 🌐 Environment Deployment Verification

## Dev Environment

**URL:** `http://44.201.216.9`

The application was successfully deployed to the Development environment.

## Staging Environment

**URL:** `http://44.211.168.120`

The application was successfully deployed to the Staging environment.

## Production Environment

**URL:** `http://13.222.41.40`

The application was successfully deployed to the Production environment after Admin approval.

# 🛠️ Infrastructure, Security & Pipeline Execution

## 1. Server Architecture & User Setup

* Jenkins Installation & Server Setup Commands
* Active AWS EC2 Instances Overview
* Configuring Security Realm & Role-Based Strategy
* User Account Directory Management
* Global Role Assignments for Users

## 2. CI/CD Pipeline & Deployments

* Creating Secure CI/CD Pipeline Job
* Pipeline Dashboard View
* Source Code Checkout Execution Log
* Interactive Quality Gate for Production Deployment Approval
* Detailed Jenkins Execution Output showing Success Status
* Complete Multi-Stage Execution View

# 📸 Project Screenshots

* Jenkins Pipeline Success
* Jenkins Pipeline Stage Graph
* Production Approval Gate
* Jenkins RBAC Configuration
* AWS EC2 Environment Isolation

# 📋 Project Deliverables

The following deliverables have been completed:

* ✅ Sample Web Application
* ✅ GitHub Repository
* ✅ Jenkinsfile
* ✅ Jenkins CI/CD Pipeline
* ✅ Build Stage
* ✅ Unit Test Stage
* ✅ Dev Deployment
* ✅ Staging Deployment
* ✅ Manual Production Approval
* ✅ Production Deployment
* ✅ Jenkins Role-Based Access Control
* ✅ Separate EC2 Instances
* ✅ Environment Promotion Strategy
* ✅ Deployment Verification
* ✅ Security Improvements Documentation
* ✅ Project Screenshots

# ✅ Final Outcome

This project successfully implements a secure CI/CD pipeline with Role-Based Access Control and a controlled environment promotion strategy.

The final deployment workflow is:

```text
GitHub
   ↓
Jenkins
   ↓
Build
   ↓
Unit Test
   ↓
Dev
   ↓
Staging
   ↓
Admin Approval
   ↓
Production
```

The implementation ensures that:

* Developers cannot directly approve Production deployments.
* Production deployment requires Admin approval.
* Dev, Staging, and Production environments are isolated.
* Application deployments are automated through Jenkins.
* Environment promotion follows a controlled Dev → Staging → Production workflow.
* Deployment activity can be tracked through Jenkins pipeline execution history.

# 🎉 Project Status

## ✅ Completed Successfully
