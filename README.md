🚀 Node.js CI/CD Pipeline with AWS EC2, Docker & GitHub Actions
📌 Project Overview

This project demonstrates a complete CI/CD pipeline for a Node.js application using:

GitHub Actions for automation
Docker for containerization
Amazon EC2 for deployment
Amazon ECR for image storage
SSH for remote deployment

The pipeline automatically builds, pushes, and deploys the application whenever code is pushed to the main branch.

🏗️ Architecture
Developer → GitHub → GitHub Actions → Docker Build → Amazon ECR → EC2 (Linux) → Running Container
⚙️ Tech Stack
Node.js
Docker
GitHub Actions
AWS EC2 (Amazon Linux 2023)
Amazon ECR
SSH (appleboy/ssh-action)
📁 Project Structure
.
├── app/
│   ├── server.js
│   ├── package.json
│   └── Dockerfile
│
├── terraform/ (optional infra setup)
│
├── .github/workflows/
│   └── ci-cd.yml
│
└── README.md
🔄 CI/CD Workflow
1. Code Push

Developer pushes code to GitHub (main branch).

2. GitHub Actions Trigger

Workflow automatically starts.

3. Docker Build

Application is containerized using Docker.

4. Push to Amazon ECR

Docker image is pushed to AWS registry.

5. SSH Deployment to EC2

GitHub Actions connects to EC2 and:

Pulls Docker image
Stops old container
Runs new container
⚠️ Errors Faced & Fixes Applied (IMPORTANT SECTION)
❌ 1. Dockerfile not found in GitHub Actions
Error:
failed to read dockerfile: open Dockerfile: no such file or directory
Cause:

GitHub Actions was searching in wrong directory (./app mismatch).

Fix:
Corrected Docker build path:
docker build -t nodejs-app ./app
❌ 2. YAML syntax error in workflow
Error:
Invalid workflow file on line 8 / 16
Cause:

Incorrect indentation in GitHub Actions YAML.

Fix:
Fixed indentation for steps:
steps:
  - name: Checkout code
    uses: actions/checkout@v4
❌ 3. SSH Authentication failed
Error:
ssh: handshake failed: unable to authenticate (publickey)
Cause:
Wrong or missing EC2 SSH key in GitHub Secrets
Incorrect key format
Fix:
Recreated EC2 key pair
Updated GitHub Secret:
EC2_SSH_KEY (full .pem content)
EC2_HOST
EC2_USER = ec2-user
❌ 4. Docker not found on EC2
Error:
docker: command not found
Cause:

Docker not installed on EC2 instance.

Fix:

Installed Docker on EC2:

sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo systemctl enable docker
sudo usermod -aG docker ec2-user
❌ 5. GitHub Actions Node.js 20 warning
Warning:
Node.js 20 actions are deprecated
Fix:
Updated actions to v4:
actions/checkout@v4
aws-actions/configure-aws-credentials@v4
❌ 6. ECR repository not found
Error:
The repository does not exist in registry
Cause:

ECR repo not created before push.

Fix:
Created ECR repository manually in AWS
Verified repository name matches workflow
❌ 7. Submodule / app folder confusion
Issue:
fatal: Pathspec 'app/Dockerfile' is in submodule
Fix:
Removed submodule confusion
Ensured app/ is normal directory
🔐 GitHub Secrets Used
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_ACCOUNT_ID
EC2_HOST
EC2_USER
EC2_SSH_KEY
🐳 Docker Setup
Build Image
docker build -t nodejs-app ./app
Run Locally
docker run -d -p 80:3000 nodejs-app
☁️ AWS Setup
EC2 Configuration
Amazon Linux 2023
Port 22 (SSH)
Port 80 (HTTP)
ECR
Private Docker registry
Stores application images
🚀 Final CI/CD Flow
Push Code → GitHub Actions → Docker Build → Push to ECR → SSH EC2 → Pull Image → Run Container → App Live
🎯 Key Learning Outcomes
CI/CD pipeline automation
Docker containerization
AWS EC2 deployment
GitHub Actions workflow debugging
SSH-based remote deployment
Real-world DevOps troubleshooting
🌐 Final Result

Application successfully:

Builds automatically
Pushes Docker image to AWS ECR
Deploys to EC2
Runs on public IP
💡 Author harin navis

DevOps Project – Automated Node.js Deployment Pipeline using AWS & GitHub Actions
