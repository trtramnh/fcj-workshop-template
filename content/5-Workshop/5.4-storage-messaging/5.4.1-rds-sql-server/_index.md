---
title: "Amazon RDS SQL Server"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---


<!-- TODO: Insert screenshot of Amazon RDS Database showing the Endpoint here -->
![RDS Database](/images/5-Workshop/placeholder-rds.png)

> [!NOTE]
> **Architecture Diagram Difference:** In the overall architecture diagram, the RDS system is depicted following Production standards with a **Multi-AZ (Primary & Standby)** mechanism for fault tolerance. However, for the scope of this hands-on Workshop, we will only provision a **Single-AZ (Free Tier)** instance to optimize and save on demo costs.

The Snaptics project uses C# with Entity Framework Core, making Microsoft SQL Server the most optimal choice.

### 1. Create DB Subnet Group
RDS requires you to group Subnets together so it knows where the Database can be placed.
- Open **Amazon RDS ➔ Subnet groups ➔ Create DB subnet group**.
- **Name:** `snaptics-db-subnet-group`
- **VPC:** Select `snaptics-vpc`
- **Subnets:** Under Add subnets, select the 2 Availability Zones `ap-southeast-1a` and `ap-southeast-1b`, then check the 2 **Private Subnets** (CIDR `10.0.3.0/24` and `10.0.4.0/24`).

### 2. Create RDS Database
- Open **RDS ➔ Databases ➔ Create database**.
- **Engine options:** Select **Microsoft SQL Server** (Express Edition if you want to use the Free Tier).
- **Templates:** Free tier (Or Production if you have budget).
- **Settings:**
  - DB instance identifier: `snaptics-db`
  - Master username: `admin`
  - Master password: `Snaptics@StrongPass123!` (Remember this password).
- **Instance configuration:** `db.t3.micro`.
- **Storage:** 20 GB General Purpose SSD (gp2).
- **Connectivity:**
  - VPC: Select `snaptics-vpc`.
  - Subnet group: Select `snaptics-db-subnet-group`.
  - **Public access:** Select **No** (Mandatory).
  - VPC security group: Choose **Choose existing** and select `snaptics-rds-sg`.
- Scroll to the bottom and click **Create database**.

The SQL Server initialization process on AWS takes about 15-20 minutes. After the status changes to **Available**, click on the database and copy the **Endpoint** string (e.g., `snaptics-db.cx1y2z3.ap-southeast-1.rds.amazonaws.com`).
