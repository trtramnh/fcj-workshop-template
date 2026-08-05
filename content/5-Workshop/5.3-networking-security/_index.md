---
title: "Networking & Security"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---


In an Enterprise architecture, the networking layer is highly complex. We must secure incoming traffic at the edge and distribute it via CloudFront, while optimizing internal backend traffic (VPC Endpoints) to reduce bandwidth costs.

## 1. Custom Domain with Route 53 (Optional)

If you own a domain (e.g., `snaptics.com`), you should use Route 53 to route traffic.
- Open **Route 53 ➔ Hosted zones ➔ Create hosted zone**.
- Enter your domain name. Update your domain registrar's Name Servers (NS) to match the ones provided by Route 53.
- Request a free SSL certificate via **AWS Certificate Manager (ACM)** in the `us-east-1` region (Required for CloudFront).

## 2. Content Delivery Network (CloudFront)

Instead of exposing the Application Load Balancer (ALB) directly to the world, we hide it behind CloudFront to optimize performance and hide the real IP. *(Note: WAF is already integrated automatically when we host the Frontend with AWS Amplify, so we don't need to configure an external WAF here).*

### Configure Amazon CloudFront for API
- Open **CloudFront ➔ Create Distribution**.
- **Origin domain:** Select your Application Load Balancer (which we will create in the Compute section).
- **Viewer Protocol Policy:** Redirect HTTP to HTTPS.
- **Cache key and origin requests:** Choose **Cache policy and origin request policy** ➔ CachingDisabled and AllViewer (mandatory for dynamic APIs).
- **Custom SSL Certificate:** Attach the ACM certificate you created.
- Click Create.

## 3. Multi-Tier VPC & Subnets

Create the internal network (`snaptics-vpc`) with CIDR `10.0.0.0/16`.

Create 4 subnets in `ap-southeast-1`:
1. `snaptics-public-1a`: `10.0.1.0/24` (Enable auto-assign public IPv4)
2. `snaptics-public-1b`: `10.0.2.0/24` (Enable auto-assign public IPv4)
3. `snaptics-private-1a`: `10.0.3.0/24` (For ECS & SQL Server)
4. `snaptics-private-1b`: `10.0.4.0/24` (For ECS & SQL Server)

## 4. Gateways & VPC Endpoints

### A. Internet Gateway & NAT Gateway
- Create **Internet Gateway** `snaptics-igw` and attach it to the VPC.
- Create **NAT Gateway** `snaptics-nat-gw` in `snaptics-public-1a` with an Elastic IP. This allows our ECS containers to call Google Gemini and Azure APIs out on the internet.
- Configure Route Tables so Public subnets route `0.0.0.0/0` to IGW, and Private subnets route `0.0.0.0/0` to NAT Gateway.

### B. VPC Gateway Endpoint for S3 (Cost Optimization)
If the ECS container uploads heavy invoice images to S3 via the NAT Gateway, AWS will charge massive data processing fees. To fix this, we use a VPC Endpoint.
- Open **VPC ➔ Endpoints ➔ Create endpoint**.
- Service category: **AWS services**.
- Service: Search for `s3` and select the **Gateway** type (`com.amazonaws.ap-southeast-1.s3`).
- VPC: `snaptics-vpc`.
- Route tables: Check the **Private Route Table**.
- Policy: **Full Access**.
- Click Create.
Now, all traffic from ECS to S3 will stay inside the AWS backbone network, which is faster and 100% Free!

## 5. Security Groups (Virtual Firewalls)

Strictly control traffic flow using Security Groups:

- **ALB Security Group (`snaptics-alb-sg`):** 
  - Allow HTTP (80) and HTTPS (443).
  - *Advanced:* You can restrict the source IP to only CloudFront prefix lists, completely blocking direct internet access to the ALB!
- **ECS Security Group (`snaptics-ecs-sg`):**
  - Allow Custom TCP `8080` ONLY from `snaptics-alb-sg`.
- **SQL Server Security Group (`snaptics-aurora-sg`):**
  - Allow MySQL/SQL Server (3306) or PostgreSQL (5432) ONLY from `snaptics-ecs-sg`.
