---
title: "Tech Stack & AWS Services"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.1.2. </b> "
---


To operate a smooth financial system like Snaptics, we use a hybrid ecosystem combining .NET Core and AWS Managed Services. Below is a breakdown of the role of each technology in the project:

### Application Core

- **C# .NET 8 / 9:** A powerful framework used to build the Web API. High performance and strict security thanks to static typing, highly suitable for financial applications.
- **Entity Framework Core (EF Core):** The primary ORM to communicate with the SQL Server database. Manages mapping Models (like `Transactions`, `Invoices`) into database tables.
- **Hangfire:** Background job processing library. Used to schedule tasks like end-of-month spending calculation and budget rollover to the new month.
- **SignalR:** .NET WebSockets technology that helps push real-time notifications to mobile devices when an invoice is fully scanned.

### AWS Infrastructure

- **Amazon ECS Fargate:** Packages the .NET Backend API and AI Worker into Docker containers running on ECS Cluster. Fargate automatically runs containers across Private Subnets in 02 Availability Zones without managing EC2 instances.
- **Amazon Aurora & RDS (Primary / Standby):** Fully managed relational database service. Supports Multi-AZ replication (Primary in AZ 2, Standby in AZ 1), automated backups, and hardware fault tolerance.
- **Amazon S3 (Simple Storage Service):** Object storage for receipt images and data files. Connects directly via **S3 Gateway Endpoint** within the VPC to optimize bandwidth costs and security.
- **AWS Amplify:** Hosting platform and automated build/deploy pipeline for the Frontend SPA from the GitHub Repository.
- **Amazon CloudFront & Route 53:** Route 53 manages DNS; CloudFront serves as CDN delivering Frontend assets and routing API Requests through the Internet Gateway to ALB.

### Messaging & Integration

- **Amazon SQS (`snaptics-ai-queue`) & DLQ:** Message queue buffering asynchronous OCR/AI tasks, paired with Dead Letter Queue (DLQ) to hold failed messages for inspection and replay.
- **Amazon SNS (Simple Notification Service):** Dispatches operational alerts and system health notifications.
- **AWS Secrets Manager:** Securely stores and injects sensitive configuration parameters (RDS Connection String, Gemini API Key, Azure Credentials, JWT Secret) into Fargate Tasks.

### Artificial Intelligence (AI & OCR)

- **Google Gemini API (Vision):** Smart financial image processing and Q&A (e.g., "What did I spend the most on this month?").
- **Azure Document Intelligence:** Microsoft's extremely powerful OCR tool that accurately extracts static data fields from invoices (Merchant Name, Tax ID, Total Amount, Date & Time) entirely automatically.
