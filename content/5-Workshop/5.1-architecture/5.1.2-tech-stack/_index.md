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

- **Amazon ECS Fargate:** Packages the above .NET application into a container (Docker). Fargate helps run this container without the effort of configuring or maintaining underlying EC2 servers. Scales out automatically during traffic spikes.
- **Amazon RDS (SQL Server):** Fully managed Database service. Supports auto-backup, security patching, and Multi-AZ to ensure hardware fault tolerance.
- **Amazon S3 (Simple Storage Service):** Cheap and infinitely durable Object Storage, used to hold all user image files and PDF receipts before sending them for OCR analysis.

### Messaging & Integration

- **Amazon SNS (Simple Notification Service):** Primarily used to fire emergency alerts or stream events to multiple subscribers simultaneously.
- **Amazon SQS (Simple Queue Service):** Message queue. Acts as a buffer when invoice upload volume is too high, preventing system bottlenecks (Decoupling).
- **AWS Parameter Store:** A safe place to store sensitive configuration parameters. The .NET application will automatically load configs from here at startup to avoid exposing Secret Keys in the source code.

### Artificial Intelligence (AI & OCR)

- **Google Gemini API (Vision):** Smart financial image processing and Q&A (e.g., "What did I spend the most on this month?").
- **Azure Document Intelligence:** Microsoft's extremely powerful OCR tool that accurately extracts static data fields from invoices (Merchant Name, Tax ID, Total Amount, Date & Time) entirely automatically.
