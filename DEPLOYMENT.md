# AWS EC2 Deployment Guide

## Prerequisites

1. AWS Account with EC2 and ECR access
2. GitHub repository with this code
3. EC2 instance running (Ubuntu 22.04 recommended)
4. Docker installed on EC2

## AWS Setup

### 1. Create ECR Repository
```bash
aws ecr create-repository --repository-name construction-genai
```

### 2. Create IAM User for CI/CD
- Create IAM user with `AmazonEC2ContainerRegistryFullAccess` policy
- Generate Access Key and Secret Key
- Store in GitHub Secrets (see below)

### 3. Configure EC2 Security Group
- Allow inbound: SSH (22), HTTP (80), HTTPS (443), Custom (8501)
- Source: Your IP for SSH, 0.0.0.0/0 for HTTP/HTTPS/8501

### 4. Setup EC2 Instance
```bash
# SSH into EC2
sudo apt update
sudo apt install docker.io -y
sudo usermod -aG docker $USER
# Logout and login back
```

## GitHub Secrets Configuration

Add these secrets in your GitHub repository settings:

- `AWS_ACCESS_KEY_ID`: IAM user access key
- `AWS_SECRET_ACCESS_KEY`: IAM user secret key
- `EC2_HOST`: EC2 public IP or DNS
- `EC2_SSH_KEY`: Private SSH key content (full key file)
- `GOOGLE_API_KEY`: Your Google AI API key

## Deployment

1. Push code to main branch
2. GitHub Actions automatically:
   - Builds Docker image
   - Pushes to ECR
   - Deploys to EC2

## Access Application

```
http://<your-ec2-public-ip>:8501
```

## Manual Deployment (if needed)

```bash
# From EC2 instance
docker pull <ecr-uri>/construction-genai:latest
docker stop construction-genai
docker rm construction-genai
docker run -d --name construction-genai -p 8501:8501 --restart unless-stopped <ecr-uri>/construction-genai:latest
```
