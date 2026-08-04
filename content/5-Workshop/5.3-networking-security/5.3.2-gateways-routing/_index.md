---
title: "Gateways & Routing"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---


Private Subnets are essentially closed networks; containers inside (like Snaptics ECS) cannot proactively call out to Google Gemini API or download libraries without an exit route (Outbound traffic). The solution is to use a NAT Gateway.

### 1. Internet Gateway (IGW)
The IGW is the gateway connecting the entire VPC to the Internet.
- Go to **VPC ➔ Internet Gateways ➔ Create internet gateway**.
- **Name:** `snaptics-igw`.
- Select the created IGW ➔ **Actions ➔ Attach to VPC** ➔ Choose `snaptics-vpc`.

### 2. NAT Gateway
The NAT Gateway acts as an intermediary, receiving requests from the Private Subnet, calling out to the Internet on its behalf via the IGW, and returning the results.
- Go to **VPC ➔ NAT Gateways ➔ Create NAT gateway**.
- **Name:** `snaptics-nat-gw`
- **Subnet:** Select `snaptics-public-1a` (NAT must be placed in a Public network).
- **Elastic IP allocation ID:** Click **Allocate Elastic IP** to purchase an AWS static IP for the NAT.

### 3. Route Tables
We need to direct the Subnets on where to send traffic.

**Create Route Table for Public Subnets:**
- Create a Route Table named `snaptics-public-rt`.
- Go to **Routes ➔ Edit routes**: Point Destination `0.0.0.0/0` to Target `snaptics-igw`.
- Go to **Subnet associations**: Associate it with the 2 networks `snaptics-public-1a` and `snaptics-public-1b`.

**Create Route Table for Private Subnets:**
- Create a Route Table named `snaptics-private-rt`.
- Go to **Routes ➔ Edit routes**: Point Destination `0.0.0.0/0` to Target `snaptics-nat-gw`.
- Go to **Subnet associations**: Associate it with the 2 networks `snaptics-private-1a` and `snaptics-private-1b`.
