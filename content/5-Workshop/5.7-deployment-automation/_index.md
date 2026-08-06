---
title: "CI/CD Automation"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---


Forget manual deployment scripts! In a true Enterprise architecture, the developer only needs to write code and commit to GitHub. **GitHub Actions** will completely automate the build, test, and deployment processes for both the Backend (.NET) and the Frontend (Angular).

## 1. Configuring GitHub Secrets

GitHub needs your AWS credentials to push Docker images and deploy to Amplify.
1. Open your Snaptics repository on GitHub.
2. Go to **Settings ➔ Secrets and variables ➔ Actions**.
3. Click **New repository secret**. Add the following:
   - `AWS_ACCESS_KEY_ID`: Paste the Access Key ID from the `github-actions-snaptics` user.
   - `AWS_SECRET_ACCESS_KEY`: Paste the Secret Key.
   - `AWS_REGION`: `ap-southeast-1`

## 2. Backend CI/CD Pipeline (ECS Fargate)

Create a YAML file inside your repository at `.github/workflows/deploy.yml`:

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
        echo "Building docker image..."
        docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
        echo "Pushing image to ECR..."
        docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG

    - name: Force New Deployment in Fargate
      run: |
        echo "Telling ECS to fetch the new image..."
        aws ecs update-service --cluster ${{ env.ECS_CLUSTER }} --service ${{ env.ECS_SERVICE }} --force-new-deployment --region ${{ env.AWS_REGION }}
```

### Zero-Downtime Deployment
The final command `--force-new-deployment` executes an intelligent **Rolling Update**. The ALB routes traffic to the old containers while the new ones boot up. Once the new containers return a healthy status, the ALB instantly redirects all traffic to them and safely drains the old ones. Your users will experience exactly zero downtime!

## 3. Frontend CI/CD Pipeline (AWS Amplify)

AWS Amplify makes hosting Angular SPAs incredibly easy. Instead of writing a complex YAML file for Amplify, AWS provides direct GitHub integration.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.7-deployment-automation/amplify_create_1.png" >
  </div>
    <div> <img src="/fcj-workshop-template/images/5-Workshop/5.7-deployment-automation/amplify_create_1.2.png" >
  </div>
    <div> <img src="/fcj-workshop-template/images/5-Workshop/5.7-deployment-automation/amplify_create_1.3.png" >
  </div>
    <div> <img src="/fcj-workshop-template/images/5-Workshop/5.7-deployment-automation/amplify_create_1.4.png" >
  </div>

### Setting up Amplify Auto-Deploy
1. Go to the AWS Console, open **AWS Amplify**.
2. Click **Create new app** ➔ Select **GitHub**.
3. Authenticate with GitHub and select your `Snaptics` repository.
4. Point to the `Frontend` branch or folder.
5. Amplify will auto-detect the Angular build settings (e.g., `npm run build`).
6. Click **Save and Deploy**.

From now on, whenever a developer pushes a frontend change to the repository, Amplify automatically catches the webhook, rebuilds the site, and pushes the new assets to its edge CDN instantly.