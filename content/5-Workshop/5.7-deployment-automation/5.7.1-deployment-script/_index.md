---
title: "Deployment Script"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---


Snaptics possesses an automated PowerShell script named `deploy.ps1` located in the root directory. Whenever you complete a feature and want to push it to the real system, you only need to type a single command:

```powershell
.\deploy.ps1
```

### Script Content and Explanation

Below is the verbatim script snippet and the purpose of each command block:

```powershell
# [1/5] Define environment variables
$ECR_REGISTRY = "<ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com"
$IMAGE_NAME = "snaptics-api"
$REGION = "ap-southeast-1"
$CLUSTER_NAME = "Snaptics-Cluster"
$SERVICE_NAME = "snaptics-backend-service"

Write-Host "=========================================="
Write-Host "🚀 STARTING SNAPCTICS-API DEPLOYMENT PROCESS"
Write-Host "=========================================="

# [2/5] Log into AWS ECR via security token
Write-Host "[1/4] Logging into AWS ECR..."
aws ecr get-login-password --region $REGION | docker login --username AWS --password-stdin $ECR_REGISTRY

# [3/5] Call command to build Dockerfile
Write-Host "[2/4] Building the latest Docker image..."
docker build -t ${IMAGE_NAME}:latest .

# [4/5] Tag and Push Image to the Cloud
Write-Host "[3/4] Pushing image to AWS ECR..."
docker tag ${IMAGE_NAME}:latest ${ECR_REGISTRY}/${IMAGE_NAME}:latest
docker push ${ECR_REGISTRY}/${IMAGE_NAME}:latest

# [5/5] Extremely important: Command ECS to reload new code
Write-Host "[4/4] Commanding ECS to update Service (Force New Deployment)..."
aws ecr update-service --cluster $CLUSTER_NAME --service $SERVICE_NAME --force-new-deployment --region $REGION

Write-Host "=========================================="
Write-Host "🎉 DEPLOY COMMAND SENT SUCCESSFULLY!"
Write-Host "AWS is updating in the background. Please wait 2-3 minutes."
Write-Host "=========================================="
```
Thanks to this script, the time to release a new feature (Time-to-market) is reduced from 10 minutes to just 30 seconds!
