---
title: "Compute & Load Balancing (ECS)"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---


Snaptics backend operates on a Serverless Container architecture. We package the `.NET` application into a Docker container and run it on Amazon ECS Fargate.

## 1. Application Load Balancer (ALB)

Since the containers run in Private Subnets, we need an ALB in the Public Subnets to act as the traffic distributor.

### A. Create Target Group
- Open **EC2 ➔ Target Groups ➔ Create target group**.
- **Target type:** Select **IP addresses** (Required for Fargate `awsvpc` networking).
- **Target group name:** `snaptics-ecs-tg`.
- **Protocol / Port:** `HTTP / 8080`.
- **VPC:** `snaptics-vpc`.
- Leave targets blank (ECS will auto-register them later) and click Create.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/alb_tg_create.jpg" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/alb_tg_create_2.jpg" >
  </div>

### B. Create ALB
- Open **EC2 ➔ Load Balancers ➔ Create Load Balancer ➔ Application Load Balancer**.
- **Name:** `snaptics-alb`.
- **Scheme:** **Internal** OR **Internet-facing**. Since we have CloudFront in front of it, it can technically be Internal if configured with advanced routing, but for simplicity, we keep it **Internet-facing**.
- **Network mapping:** Select `snaptics-vpc` and check the 2 **Public Subnets**.
- **Security groups:** Apply `snaptics-alb-sg`.
- **Listeners and routing:** Add HTTP (80) and forward traffic to `snaptics-ecs-tg`.
- Click Create.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/alb_create_1.jpg" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/alb_create_1.2.jpg" >
  </div>

## 2. Amazon Elastic Container Registry (ECR)

Before creating the ECS Cluster, we need a place to store our Docker Images.
- Open **Amazon ECR ➔ Repositories ➔ Create repository**.
- **Visibility settings:** Private.
- **Repository name:** `snaptics-api`.
- Click Create. Copy the **URI** (e.g., `123456789.dkr.ecr.ap-southeast-1.amazonaws.com/snaptics-api`).

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/ecr_create_1.jpg" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/ecr_create_1.2.jpg" >
  </div>

*(Note: We don't push images manually here. GitHub Actions will do this in the next section).*

## 3. ECS Cluster & Task Definition

### A. Create Cluster
- Open **Amazon ECS ➔ Clusters ➔ Create Cluster**.
- **Name:** `Snaptics-Cluster`.
- **Infrastructure:** AWS Fargate.
- Enable **Container Insights** (This activates advanced CloudWatch monitoring as seen in the architecture diagram).

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/ecs_cluster_create.jpg" >
  </div>

### B. Task Definition
- Open **Task definitions ➔ Create new task definition**.
- **Family:** `snaptics-api-task`.
- **Infrastructure:** Fargate (Linux/X86_64).
- **CPU:** `0.5 vCPU`.
- **Memory:** `2 GB` (Provides enough headroom for `.NET` and Hangfire).
- **Task role:** Select `snaptics-ecs-task-role`.
- **Task execution role:** Select `ecsTaskExecutionRole`.
- **Container - 1:**
  - Name: `snaptics-app`
  - Image URI: Paste the ECR URI you copied earlier. *(It will fail to boot initially since the image doesn't exist yet, but GitHub Actions will fix this).*
  - Container port: `8080`.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/task_definition_create.jpg" >
  </div>


### C. Deploy ECS Service
- Go to `Snaptics-Cluster` ➔ **Services** tab ➔ **Create**.
- **Launch type:** Fargate.
- **Service name:** `snaptics-backend-service`.
- **Desired tasks:** `2` (Runs 2 containers across the 2 private subnets for High Availability).
- **Networking:** Select the 2 **Private Subnets**. Turn **OFF** Public IP. Apply `snaptics-ecs-sg`.
- **Load balancing:** Choose the ALB `snaptics-alb` and target group `snaptics-ecs-tg` created above.
- Click Create.

The service will try to start tasks but will fail because the ECR repository is currently empty. Move to the next section to unleash the power of GitHub Actions CI/CD!