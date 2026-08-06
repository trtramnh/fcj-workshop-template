---
title: "Networking & Security"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---


In an Enterprise architecture, the networking layer is highly complex. We must secure incoming traffic at the edge, while optimizing internal backend traffic (VPC Endpoints) to reduce bandwidth costs.

## 1. Custom Domain with Route 53 (Optional)

If you own a domain (e.g., `snaptics.com`), you should use Route 53 to route traffic.
- Open **Route 53 ➔ Hosted zones ➔ Create hosted zone**.
- Enter your domain name. Update your domain registrar's Name Servers (NS) to match the ones provided by Route 53.



## 2. Multi-Tier VPC & Subnets

Create the internal network (`snaptics-vpc`) with CIDR `10.0.0.0/16`.

Create 4 subnets in `ap-southeast-1`:
1. `snaptics-public-1a`: `10.0.1.0/24` (Enable auto-assign public IPv4)
2. `snaptics-public-1b`: `10.0.2.0/24` (Enable auto-assign public IPv4)
3. `snaptics-private-1a`: `10.0.3.0/24` (For ECS & SQL Server)
4. `snaptics-private-1b`: `10.0.4.0/24` (For ECS & SQL Server)

## 3. Gateways & VPC Endpoints

### A. Internet Gateway & NAT Gateway

**1. Create the Internet Gateway (`snaptics-igw`):**
- Open **VPC ➔ Internet Gateways ➔ Create internet gateway**.
- Name: `snaptics-igw`.
- Click **Create internet gateway**.
- Select the newly created IGW → **Actions ➔ Attach to VPC** → choose `snaptics-vpc` → **Attach internet gateway**.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.3-networking-security/vpc_internetgateway_create_internetgateway.png" >
  </div>

**2. Create the NAT Gateway (`snaptics-nat-gw`):**
- Open **VPC ➔ NAT Gateways ➔ Create NAT gateway**.
- Name: `snaptics-nat-gw`.
- Subnet: select the **Public subnet** `snaptics-public-1a`.
- Connectivity type: **Public**.
- VPC is automatically set to `snaptics-vpc` (based on the selected subnet).
- Elastic IP: click **Allocate Elastic IP** to create a static public IP for the NAT Gateway.
- Scroll down and click **Create NAT gateway**, then wait until the status changes from **Pending** to **Available**.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.3-networking-security/vpc_natgateway.png" >
  </div>

**3. Configure Route Tables:**
- Public subnets route `0.0.0.0/0` to the Internet Gateway.
- Private subnets route `0.0.0.0/0` to the NAT Gateway.

The NAT Gateway allows our ECS containers in the Private subnets to call Google Gemini and Azure APIs out on the internet.

### B. VPC Gateway Endpoint for S3 (Cost Optimization)
If the ECS container uploads heavy invoice images to S3 via the NAT Gateway, AWS will charge massive data processing fees. To fix this, we use a VPC Endpoint so all S3 traffic stays inside the AWS backbone network (faster and 100% free).

**Lab Steps:**
- Open **VPC ➔ Endpoints ➔ Create endpoint**.
- **Name tag:** `snaptics-s3-endpoint`.
- **Type:** AWS service.
- **Service:** In the service search box, type `s3`, then select `com.amazonaws.ap-southeast-1.s3` which has the type **Gateway**.
- **VPC:** select `snaptics-vpc`.
- **Route tables:** check the **Private Route Table**.
- **Policy:** **Full Access**.
- Scroll down and click **Create endpoint**.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.3-networking-security/vpc_endpoint_s3.png" >
  </div>

## 4. Security Groups (Virtual Firewalls)

Strictly control traffic flow using Security Groups:

- **ALB Security Group (`snaptics-alb-sg`):** 
  - Allow HTTP (80) and HTTPS (443).

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.3-networking-security/alb-sg.png" >
  </div>
- **ECS Security Group (`snaptics-ecs-sg`):**
  - Allow Custom TCP `8080` ONLY from `snaptics-alb-sg`.
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.3-networking-security/fargate_sg.png" >
  </div>
- **SQL Server Security Group (`snaptics-aurora-sg`):**
  - Allow SQL Server (1433) ONLY from `snaptics-ecs-sg`.
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.3-networking-security/db_sg.png" >
  </div>



