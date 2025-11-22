# iamdeepaktiwari08-AWS-Portfolio-cicd
# 🌐 Cloud & DevOps Portfolio Website with AWS CI/CD Pipeline

This project hosts my personal Cloud & DevOps portfolio website on AWS using a fully automated CI/CD pipeline.  
Every code change pushed to GitHub automatically gets deployed to AWS S3 and served globally through CloudFront.

---

## 🚀 Architecture Overview

### Flow Summary:
1. I push portfolio updates to GitHub
2. GitHub Actions pipeline is triggered
3. Pipeline builds & prepares static files
4. Website files are deployed to AWS S3 bucket
5. CloudFront distributes content globally
6. Route53 maps my custom domain
7. Users access the live website securely

---

## 🏗️ Architecture Diagram

![Architecture](architecture.png)

---

## 🧱 AWS Services Used

| Service | Purpose |
|---------|---------|
| **S3 Bucket** | Host website |
| **CloudFront CDN** | Global fast distribution |
| **Route53** | Domain management |
| **IAM** | Access permissions for GitHub |
| **Certificate Manager** | SSL / HTTPS |

---

## ⚙ DevOps Tools Used

| Tool | Purpose |
|------|---------|
| **GitHub Actions** | CI + CD |
| **GitHub Repository** | Version control |
| **Automated Deployments** | No manual updates |

---

---

## 🧪 CI/CD Workflow

### Trigger:
- On push to main branch

### Jobs:
- Install dependencies
- Build site
- Sync S3 bucket
- Invalidate CloudFront cache

---

## 📦 GitHub Actions Pipeline

File: `.github/workflows/deploy.yml`

```yaml
name: Deploy Portfolio to AWS

on:
  push:
    branches: [ "main" ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v4
      with:
        role-to-assume: ${{ secrets.AWS_ROLE_ARN }}
        aws-region: ap-south-1

    - name: Sync to S3
      run: |
        aws s3 sync ./src s3://${{ secrets.S3_BUCKET }} --delete

    - name: Invalidate CloudFront cache
      run: |
        aws cloudfront create-invalidation \
        --distribution-id ${{ secrets.CLOUDFRONT_ID }} \
        --paths "/*"
