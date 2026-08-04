---
title: "ECS Cluster & Task Definition"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.6.3. </b> "
---



<!-- TODO: Insert screenshot of Amazon ECS Cluster showing running Fargate tasks here -->
![ECS Fargate](/images/5-Workshop/placeholder-ecs.png)

Once the application is an Image on ECR and the Load Balancer is ready to distribute the load, we will create the Serverless server to run that Image.

### 1. Create Cluster
- Open **Amazon ECS ➔ Clusters ➔ Create Cluster**.
- **Cluster name:** `Snaptics-Cluster`.
- **Infrastructure:** Select AWS Fargate (Serverless).

### 2. Declare Task Definition
The Task Definition is like a "Blueprint" instructing ECS on which Image to run, and how much RAM and vCPU is needed.
- Open **Task definitions ➔ Create new task definition**.
- **Task definition family:** `snaptics-api-task`.
- **Infrastructure requirements:** Fargate (Linux/X86_64).
  - CPU: `1 vCPU`
  - Memory: `2 GB` (Enough for .NET + Hangfire to run smoothly).
  - **Task role:** Select `snaptics-ecs-task-role` (Allows code in the container to call S3, SQS).
  - **Task execution role:** Select `ecsTaskExecutionRole`.
- **Container - 1:**
  - Name: `snaptics-app`
  - Image URI: Paste your ECR image link here (e.g., `<ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/snaptics-api:latest`).
  - Container port: `8080`.
- Click **Create**.

### 3. Deploy Service
The Service is the orchestrator ensuring your application always has X running copies (Tasks). It also connects the application to the Load Balancer.
- Go to `Snaptics-Cluster` ➔ Tab **Services** ➔ **Create**.
- **Environment:** Launch type ➔ Fargate.
- **Deployment configuration:**
  - Application type: `Service`
  - Family: `snaptics-api-task`
  - Service name: `snaptics-backend-service`
  - Desired tasks: `2` (Run 2 containers for fault tolerance).
- **Networking:**
  - VPC: `snaptics-vpc`
  - Subnets: Check **2 Private Subnets** (`snaptics-private-1a` and `1b`).
  - Security group: Select **Use an existing** ➔ `snaptics-ecs-sg`.
  - **Public IP:** Turn off. Since Fargate is in Private, it is not allowed to have a Public IP.
- **Load balancing:**
  - Type: Application Load Balancer
  - Container: `snaptics-app 8080:8080`
  - Select Use an existing load balancer ➔ `snaptics-alb`.
  - Target group: `snaptics-tg`.
- Click **Create**.

Please wait patiently for about 2-3 minutes. When the `Tasks` tab reports 2 tasks changing to the **RUNNING** state, you can get the domain name (DNS Name) of the ALB, paste it into the browser, and admire the result!
