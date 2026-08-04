---
title: "GitHub Actions CI/CD"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.7.3. </b> "
---


While the `deploy.ps1` script is great for manual deployments from your local machine, real production environments require automated pipelines. Snaptics utilizes **GitHub Actions** to automate the build and deployment process directly from the repository.

<!-- TODO: Insert screenshot of a successful GitHub Actions run here -->
![GitHub Actions Run](/images/5-Workshop/placeholder-actions.png)

### The Workflow File

Whenever code is pushed to the `main` or `master` branch, GitHub Actions triggers the deployment pipeline. The workflow file is stored at `.github/workflows/deploy.yml` in the Backend repository:

```yaml
name: Deploy Backend to Fargate

on:
  push:
    branches:
      - main
      - master

env:
  AWS_REGION: ap-southeast-1
  ECR_REPOSITORY: snaptics-api
  ECS_SERVICE: snaptics-backend-service
  ECS_CLUSTER: Snaptics-Cluster

jobs:
  deploy:
    name: Build and Deploy
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v4

    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v4
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ env.AWS_REGION }}

    - name: Login to Amazon ECR
      id: login-ecr
      uses: aws-actions/amazon-ecr-login@v2

    - name: Build, tag, and push image to ECR
      id: build-image
      env:
        ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
        IMAGE_TAG: latest
      run: |
        docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
        docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG

    - name: Force New Deployment in Fargate
      run: |
        aws ecs update-service --cluster ${{ env.ECS_CLUSTER }} --service ${{ env.ECS_SERVICE }} --force-new-deployment --region ${{ env.AWS_REGION }}
```

### Security Considerations
Notice how we securely manage the AWS credentials (`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`) using **GitHub Repository Secrets**. These keys are never hardcoded in the codebase, preventing unauthorized access to the AWS account.
