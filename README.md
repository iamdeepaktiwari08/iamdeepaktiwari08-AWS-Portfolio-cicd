# iamdeepaktiwari08-AWS-Portfolio-cicd
# 🌐 AWS Portfolio Website with GitHub Actions CI/CD

This project hosts my personal Cloud & DevOps portfolio website on AWS S3 with a fully automated CI/CD deployment pipeline using GitHub Actions.

Every code change pushed to GitHub is automatically deployed to the AWS S3 Bucket.

---

## 🚀 Architecture Overview

### Deployment Flow
1. I update code locally
2. I push changes to GitHub
3. GitHub Actions pipeline runs automatically
4. Pipeline uploads website files to S3 bucket
5. Portfolio website becomes live instantly

---

## 🏗️ Architecture Diagram

![Architecture](architecture.png)

---

## 🧱 AWS Services Used

| Service | Purpose |
|---------|----------|
| **S3 Bucket** | Host static website files |
| **IAM Role / Policy** | Permission for GitHub Actions |

---

## ⚙️ DevOps Tools Used

| Tool | Purpose |
|------|----------|
| **GitHub Repository** | Version control |
| **GitHub Actions** | CI/CD pipeline |
| **Automated Deployments** | No manual upload |

---

## 📦 GitHub Actions Workflow

`.github/workflows/deploy.yml`

```yaml
name: CI/CD Deploy to S3

on:
  push:
    branches: [ "main" ]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout source
      uses: actions/checkout@v4

    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v4
      with:
        role-to-assume: ${{ secrets.AWS_ROLE_ARN }}
        aws-region: ap-south-1

    - name: Upload to S3
      run: |
        aws s3 sync ./src s3://${{ secrets.S3_BUCKET }} --delete
