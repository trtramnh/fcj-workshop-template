---
title: "VPC & Subnets Design"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---



<!-- TODO: Insert screenshot of AWS VPC Dashboard showing your Subnets here -->
![VPC Dashboard](/images/5-Workshop/placeholder-vpc.png)

First, we create a Virtual Private Cloud acting as the "network boundary" for the entire Snaptics system.

Go to **AWS Console ➔ VPC ➔ Your VPCs ➔ Create VPC**:
- **Name tag:** `snaptics-vpc`
- **IPv4 CIDR block:** `10.0.0.0/16` (Allocates 65,536 IP addresses).

### Subnet Planning
The system is deployed Multi-AZ (Multiple Availability Zones) in the `ap-southeast-1` (Singapore) region, so we need to create 4 Subnets:

Go to **VPC ➔ Subnets ➔ Create subnet** and sequentially create:

| Subnet Name | Availability Zone | IPv4 CIDR block | Primary Function |
| :--- | :--- | :--- | :--- |
| **`snaptics-public-1a`** | `ap-southeast-1a` | `10.0.1.0/24` | Contains the Application Load Balancer and NAT Gateway. |
| **`snaptics-public-1b`** | `ap-southeast-1b` | `10.0.2.0/24` | Contains the Application Load Balancer (For high availability). |
| **`snaptics-private-1a`** | `ap-southeast-1a` | `10.0.3.0/24` | Contains ECS Fargate Task 1 and RDS Primary. |
| **`snaptics-private-1b`** | `ap-southeast-1b` | `10.0.4.0/24` | Contains ECS Fargate Task 2 and RDS Replica. |

> [!IMPORTANT]
> For the 2 `snaptics-public-1a` and `snaptics-public-1b` networks, after creation, check the Subnet ➔ Actions ➔ **Edit subnet settings** ➔ Check **Enable auto-assign public IPv4 address**.
