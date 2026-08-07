---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


Welcome to the comprehensive implementation guide for **Snaptics** – an intelligent financial management system built on the **.NET 10** and Angular/Amplify platforms

In this workshop, you will learn how to deploy a full-fledged, highly available, and secure production environment on AWS. We have upgraded the architecture from a basic deployment to an **Enterprise-grade** setup that aligns with the AWS Well-Architected Framework.

### Key Upgrades in this Architecture:
- **Global Delivery & Security:** Implementation of **Route 53** (DNS) and **ALB** for blazing fast content delivery.
- **Frontend Hosting:** Fully automated deployment of the Angular frontend using **AWS Amplify**.
- **Serverless Compute:** Running the .NET Core backend on **Amazon ECS Fargate** behind an Application Load Balancer (ALB).
- **Enterprise Database:** Migrating from basic SQL Server to **Amazon RDS for SQL Server** with Primary/Standby replication for High Availability (Multi-AZ).
- **Secure Storage & Secrets:** Replacing NAT Gateway data transfers to S3 with highly secure **VPC Gateway Endpoints**. Managing sensitive keys via **AWS Systems Manager Parameter Store**.
- **Resilient AI Messaging:** Upgrading the Amazon SQS `snaptics-ai-queue` with a **Dead Letter Queue (DLQ)** to handle failed AI processing gracefully.
- **Fully Automated CI/CD:** Abandoning manual local scripts in favor of a professional **GitHub Actions** pipeline that automatically builds, tests, and deploys Docker images to ECR and updates the ECS cluster.
- **Observability:** Centralized logging and alerting using **Amazon CloudWatch**, **SNS**, and **AWS Budgets**.

---

### Workshop Structure

To keep navigation simple while delivering deep technical insights, this workshop is flattened into 9 comprehensive modules. Please follow them in order:

1. **Overview & Architecture:** Deep dive into the Enterprise Architecture diagram and component roles.
2. **Prerequisites:** Setting up GitHub, AWS IAM Users, and API Keys for Google Gemini / Azure OCR.
3. **Networking & Security:** Creating the Multi-Tier VPC, Route 53, and VPC Endpoints.
4. **Database, Storage & Secrets:** Deploying RDS SQL Server, S3 Buckets, and AWS Systems Manager Parameter Store.
5. **Messaging & AI Integration:** Configuring the SQS `snaptics-ai-queue` with DLQ and integrating AI APIs.
6. **Compute & Backend (ECS):** Building the Docker image and orchestrating Fargate tasks via ALB.
7. **CI/CD Pipeline (GitHub Actions):** Writing YAML workflows to automate Deployments for both Amplify and ECS.
8. **System-Wide E2E Testing:** Validating API flows and Real-time WebSocket (SignalR) connections through the ALB.
9. **Resources Cleanup:** A crucial step-by-step guide to tearing down the infrastructure and avoiding unwanted AWS charges.

> [!NOTE]  
> This workshop assumes you have a basic understanding of AWS Console navigation. Grab a cup of coffee, and let's start building a production-ready system!