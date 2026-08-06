---
title: "Overview & Architecture"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---


Before getting hands-on with the AWS Console, it is crucial to thoroughly understand the architecture diagram. This Enterprise architecture applies multiple AWS best practices regarding Security, High Availability, and Cost Optimization.

## 1. Enterprise System Architecture Diagram

![Snaptics System Architecture](/fcj-workshop-template/images/5-Workshop/5.1-Workshop-overview/snaptics-architecture.png)

### Deep Dive into the Data Flow

Take a close look at the numbered black circles in the diagram above. They represent the lifecycle of a user's request:

1. **DNS Resolution (Route 53):** When a user accesses the application, the request hits **Amazon Route 53**. Route 53 routes frontend requests to **AWS Amplify** and API requests to **Application Load Balancer (ALB)**.
2. **VPC Ingress (IGW to ALB):** From Route 53, the traffic routes through the **Internet Gateway** down to the **Application Load Balancer (ALB)** residing in the `Public Subnet`.
3. **Compute Layer (ECS Fargate):** The ALB forwards the request to the `.NET Containers` running on **ECS Fargate**, safely isolated within the `Private Subnet`.
4. **Secure Storage (VPC Gateway Endpoint):** When the ECS container needs to save an uploaded invoice image to the **S3 Bucket**, it routes traffic directly through a **Gateway Endpoint**. This keeps the traffic within the internal AWS network, avoiding the expensive NAT Gateway.
5. **Database Storage (SQL Server):** Transactional data is recorded in the **Amazon RDS for SQL Server** cluster running in **Primary/Standby** mode across 2 Availability Zones for High Availability.
6. **Asynchronous Processing (SQS):** Heavy tasks like calling AI APIs are pushed into the `snaptics-ai-queue`. If a task fails repeatedly, it gets moved to a **Dead Letter Queue (DLQ)** for manual intervention.
7. **NAT Gateway Routing:** For tasks that must connect to the external Internet, the ECS Container routes traffic through the **NAT Gateway** (in the Public Subnet).
8. **Internet Egress:** The NAT Gateway forwards the traffic to the Internet Gateway.
9. **External AI Integration:** The request officially leaves the AWS Cloud, connecting to **External AI APIs** (Google Gemini, Azure Document Intelligence) for invoice reading and financial analysis.

### CI/CD Pipeline Flow 
- **Developer** commits code to the **GitHub Repo**.
- **GitHub Actions** triggers automatically.
- It builds the Docker Image and pushes it to the **Elastic Container Registry (ECR)**.
- It then executes a command to update the ECS Fargate service without any downtime.
- For the frontend, GitHub Actions triggers a build in **AWS Amplify**.

### Observability & Security
- **CloudWatch** aggregates logs from ECS and RDS.
- **AWS Systems Manager Parameter Store** securely stores database passwords and AI API keys.
- **SNS & AWS Budgets** work together to alert administrators via email if the infrastructure cost exceeds the monthly limit.

---

## 2. Tech Stack Summary

- **Frontend:** Angular/ Hosted on AWS Amplify.
- **Backend Core:** C# .NET 10 / Entity Framework Core / SignalR (WebSockets).
- **Database:** Amazon RDS for SQL Server & RDS Multi-AZ.
- **Containerization:** Docker / Amazon Elastic Container Registry (ECR).
- **Compute:** Amazon ECS (Fargate) Serverless.
- **Networking:** Route 53, ALB, VPC Endpoints.
- **CI/CD:** GitHub Actions.
- **AI Services:** Google Gemini API, Azure Document Intelligence.

---

## 3. Cost Estimation

Below is an accurate cost estimation for a Demo environment (1 month of development & demo), as well as a reference for a Production Multi-AZ expansion.

### 3.1. Demo Environment Budget (1 month)

| No. | Service Category | Basis of Estimate | Cost (USD) |
| :--- | :--- | :--- | :--- |
| 1 | **AWS Amplify & Route 53** | Build/hosting Frontend and 01 Hosted Zone | $4.50 |
| 2 | **Amazon S3** | Storing ~20 GB of invoice images and upload/download requests | $1.00 |
| 3 | **ECS Fargate - Backend & AI Worker** | Small configuration task, total ~200-220 running hours | $8.00 |
| 4 | **Application Load Balancer (ALB)** | Operating during deployment & demo, low traffic | $7.00 |
| 5 | **Amazon RDS for SQL Server** | SQL Server Express, Single-AZ, 20 GB | $20.00 |
| 6 | **NAT Gateway & Data Transfer** | 01 NAT Gateway, limited uptime during integration | $13.00 |
| 7 | **Amazon SQS, SNS & ECR** | OCR/AI queues, basic alerts and storing Docker Image | $1.00 |
| 8 | **CloudWatch, Parameter Store & Budgets**| Logs, metrics, alarms, secrets and budget alerts | $3.00 |
| 9 | **Azure Document Intelligence** | ~1,000 pages using prebuilt invoice model | $10.00 |
| 10 | **Gemini API** | Est. 1M input tokens and 200K output tokens | $0.80 |
| | **Total Estimated Demo Cost** | | **~$68.30** |

### 3.2. Production Multi-AZ Environment (Reference for Scaling)

| Service Category | Estimated Cost / Month |
| :--- | :--- |
| **ECS Fargate & Application Load Balancer (Auto Scaling)** | $60 - $150 USD |
| **SQL Server Primary/Standby (Multi-AZ)**| $150 - $300 USD |
| **Dual NAT Gateway & Data Transfer** | $70 - $120 USD |
| **S3, SQS, SNS, ECR & CloudWatch**| $20 - $60 USD |
| **External AI APIs (Azure Document Intelligence & Gemini)** | Depends on actual invoice volume |
| **Total Estimated Production Cost** | **$300 - $600 USD / month** (Excl. AI APIs) |

> [!WARNING]
> **Extremely Important:** If you are running this workshop for learning purposes on your personal account, **YOU MUST** execute the steps in the **5.9 Cleanup** section immediately after testing to destroy the resources. Leaving SQL Server Multi-AZ and NAT Gateway running will drain your credit card rapidly!






