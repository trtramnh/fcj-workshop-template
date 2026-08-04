---
title: "Security Groups"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---



<!-- TODO: Insert screenshot of AWS EC2 Security Groups here -->
![Security Groups](/images/5-Workshop/placeholder-sg.png)

A Security Group (SG) acts as an Interface-level virtual firewall. Standard architecture dictates that application layers can only communicate with their adjacent layers.

Go to **EC2 ➔ Security Groups ➔ Create security group** and strictly configure the following 3 layers:

### 1. ALB Security Group (`snaptics-alb-sg`)
This is the only gateway that catches traffic from users.
- **VPC:** `snaptics-vpc`
- **Inbound Rules:**
  - Type: `HTTP` (Port 80) | Source: `0.0.0.0/0`
  - Type: `HTTPS` (Port 443) | Source: `0.0.0.0/0`
- **Outbound Rules:** Default Allow All.

### 2. ECS Backend Security Group (`snaptics-ecs-sg`)
Protects the .NET containers. It is NOT allowed to receive direct requests from Public IPs.
- **VPC:** `snaptics-vpc`
- **Inbound Rules:**
  - Type: `Custom TCP` | Port Range: `8080` (or `80` depending on the port you EXPOSE in the Dockerfile).
  - Source: Select Custom, type in `snaptics-alb-sg` and select the ID of the ALB group above.
  *(Meaning: Only the Load Balancer has permission to call into the Container)*.
- **Outbound Rules:** Default Allow All.

### 3. RDS Security Group (`snaptics-rds-sg`)
Protects the SQL Server database.
- **VPC:** `snaptics-vpc`
- **Inbound Rules:**
  - Type: `MS SQL` | Port Range: `1433`
  - Source: Select Custom, type in `snaptics-ecs-sg` and select the ID of the ECS group above.
  *(Meaning: Only the Snaptics .NET Containers have permission to call into the Database)*.

> [!CAUTION]
> Absolutely do not open Inbound Port 1433 to `0.0.0.0/0`. Hackers worldwide continuously scan this port to execute ransomware into SQL Server.
