---
title: "VPC & Security"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---


To isolate Snaptics from threats on the Public Internet, we will build a **Multi-Tier** Virtual Private Cloud (VPC) architecture. 

The golden rule: All data processing resources (such as ECS Containers and RDS Databases) MUST reside in Private Subnets, without any Public IP addresses. They can only be accessed through an Application Load Balancer located in a Public Subnet.
