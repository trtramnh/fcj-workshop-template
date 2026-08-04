---
title: "Application Load Balancer"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.6.2. </b> "
---


Before configuring ECS, we need a Load Balancer standing outside in the Public Subnet to receive user requests and distribute the load back to the Fargate containers located in the Private network.

### 1. Create Target Group
The Target Group is a list of container IPs that the ALB will distribute traffic to.
- Go to **EC2 ➔ Target Groups ➔ Create target group**.
- **Choose a target type:** You MUST choose **IP addresses** (because Fargate uses the `awsvpc` network, each container will have its own IP).
- **Target group name:** `snaptics-tg`
- **Protocol / Port:** `HTTP` / `8080` (The port of the .NET application).
- **VPC:** `snaptics-vpc`
- At the "Register targets" step, just leave it blank and click **Create** (ECS will automatically register IPs into this Target Group later).

### 2. Initialize Load Balancer
- Go to **EC2 ➔ Load Balancers ➔ Create Load Balancer**.
- Select **Application Load Balancer (ALB)**.
- **Name:** `snaptics-alb`
- **Scheme:** `Internet-facing` (So the application can be connected from the outside).
- **Network mapping:** Select `snaptics-vpc`. Check both **Public Subnets** (`snaptics-public-1a` and `1b`).
- **Security groups:** Remove the default SG, select the `snaptics-alb-sg` group created in the previous lesson.
- **Listeners and routing:** At port `HTTP:80`, in the Default action section, choose Forward to `snaptics-tg`.
- Click **Create**.

> [!TIP]
> To run the API securely with HTTPS, you need to purchase a Domain and use the AWS Certificate Manager (ACM) service to create a free SSL certificate, then add a Listener on Port 443 on the ALB.
