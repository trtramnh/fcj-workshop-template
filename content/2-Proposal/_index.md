---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# SNAPTICS – AI-Powered Personal Expense Management and Receipt Scanning Platform
## Cloud-Native AWS Expense Management & AI Receipt Scanning Platform

---

### 1. Executive Summary

**Snaptics** is a personal and family financial management platform designed to help users record, track, and analyze expenses visually. Instead of manually inputting every single transaction, users can simply snap a photo or upload an image of a receipt. The system leverages Optical Character Recognition (OCR) and Artificial Intelligence (AI) to recognize key receipt information, including merchant name, transaction date, total amount, category, and individual line items.

Once processed, Snaptics automatically stores transactions, categorizes expenses, and updates the user's financial dashboard. The system provides interactive charts, reports, budget tracking, shared wallets, and AI-driven financial recommendations based on spending habits.

Snaptics is built as a SaaS web application deployed on AWS Cloud-Native infrastructure. The system architecture utilizes services such as **AWS Amplify**, **Amazon CloudFront**, **Amazon ECS Fargate**, **Application Load Balancer**, **Amazon SQS**, **Amazon S3**, **Amazon ECR**, **Amazon CloudWatch**, and **SQL Server** in a Primary/Standby configuration. AI capabilities are integrated with **Azure Document Intelligence**, **Gemini API**, and **OpenAI API**.

The platform serves two main user roles:
* **User**: Manages transactions, receipts, personal and family budgets, shared wallets, views analytics reports, and receives AI advice.
* **Admin**: Manages users, notifications, support tickets, system configuration, background tasks, and overall platform monitoring.

---

### 2. Project Goals

#### 2.1. General Objective
Build a smart expense management platform that reduces manual entry time, empowers users to control budgets, and provides actionable financial insights using data analytics and artificial intelligence.

#### 2.2. Specific Objectives
* **Automation**: Automatically extract receipt data via OCR; auto-create and categorize transactions based on receipt content.
* **Flexible Management**: Support manual transaction entry; manage multiple personal and family wallets/budgets; enable multi-member collaboration on shared budgets.
* **Analytics & Reporting**: Display spending reports by day, week, month, and category; analyze spending habits with AI recommendations; provide an interactive AI chat interface with message history.
* **Alerts & Notifications**: Send proactive alerts when budget limits are approached or exceeded; build a centralized notification center.
* **System Administration**: Provide an Admin Panel to monitor users, support tickets, notifications, and background processing workers.
* **Cloud Operation & Deployment**: Deploy on AWS with scalability, security, and centralized monitoring; implement CI/CD automation for testing and deployment.

---

### 3. Problem Statement

* **Manual Expense Entry**: Most users record expenses via notebooks, Excel, or manual app entry, which is time-consuming, error-prone, and frequently abandoned. Snaptics solves this by enabling image uploads and automated data extraction.
* **Fragmented Financial Data**: Expense information is scattered across paper receipts, banking apps, and e-wallets. Snaptics consolidates all transactions into a single unified platform.
* **Budget Control Challenges**: Users often realize they overspent only after budgets are breached. Snaptics tracks budget utilization in real-time and triggers proactive alerts.
* **Lack of Behavior Insights**: Raw transactions provide little value without synthesis. Snaptics applies AI to analyze historical data, detect spending trends, and offer tailored financial advice.
* **Family Expense Collaboration**: Shared household budgets lack visibility when member spending is uncoordinated. Snaptics offers family wallets and shared budget pools.
* **AI Service Scalability**: Synchronous OCR and AI processing can cause API timeouts under load. Snaptics uses Amazon SQS for asynchronous AI processing to keep Backend APIs fast and responsive.

---

### 4. Solution Architecture

Snaptics utilizes an AWS Cloud-Native architecture combining web applications, containerized microservices, asynchronous queues, high-availability databases, and external AI services.

![Snaptics AWS Cloud Architecture](/images/2-Proposal/snaptics_architecture.jpg)

#### 4.1. Key Components

* **Frontend**: Single Page Application (SPA) hosted on **AWS Amplify**. Connected directly to GitHub Repository for automated builds/deploys, integrated with **Amazon Route 53** for DNS management and **Amazon CloudFront** (CDN) for fast, secure HTTPS delivery.
* **Backend API**: Packaged as Docker Images stored on **Amazon ECR**, deployed via **AWS Fargate (Amazon ECS Cluster)** behind an **Application Load Balancer (ALB)** with **Auto Scaling** capabilities.
* **Database**: **SQL Server** deployed in a Primary/Standby model across Private Subnets in two Availability Zones (Multi-AZ), managing user accounts, transactions, receipts, wallets, budgets, notifications, AI chat history, tickets, and audit logs.
* **Media Storage**: **Amazon S3** stores raw receipt images, transaction attachments, and processed images, decoupling binary assets from database storage.
* **OCR & AI Pipeline**:
  - Backend uploads images to S3 and enqueues messages to **Amazon SQS** (`snaptics-ai-queue`).
  - **AI Worker** on ECS Fargate dequeues messages, invokes **Azure Document Intelligence** for OCR extraction, then sends structured data to **Gemini API** / **OpenAI API** for classification & insights.
  - Failed messages are redirected to a **Dead Letter Queue (DLQ)** for debugging and fault handling.
* **Notification System**: Managed centrally in DB combined with **Amazon SNS** for pushing alerts (budget warnings, OCR status, AI tips, system updates).
* **Security**: Secrets stored in **AWS Systems Manager Parameter Store**, Private Subnets for Backend/DB, mandatory HTTPS, Access Token authentication, Admin/User RBAC, encrypted storage, and Audit Logging.
* **CI/CD Pipeline**: Automated via **GitHub Actions** (Docker build, ECR push, ECS service update) and **AWS Amplify** (automatic web frontend builds).
* **Monitoring & Cost Control**: **Amazon CloudWatch** (logs, container metrics, SQS depth) and **AWS Budgets** (cost alerts at 50%, 80%, 100% thresholds).

#### 4.2. Receipt Processing Flow
1. User uploads a receipt image via the Frontend.
2. Frontend sends image to Backend API -> Backend saves image to **Amazon S3**.
3. Backend pushes a processing message to **Amazon SQS**.
4. **AI Worker** retrieves message from SQS, calls **Azure Document Intelligence** for text/table extraction.
5. AI Worker sends extracted data to **Gemini API / OpenAI API** for category normalization and transaction parsing.
6. AI Worker saves transaction records into **SQL Server** and generates user notifications.
7. Frontend Dashboard and Financial Reports automatically update.

---

### 5. Project Timeline (12 Weeks)

| Phase | Duration | Core Tasks | Expected Outcomes |
| :--- | :--- | :--- | :--- |
| **Phase 1** | Weeks 1–2 | Requirement analysis, Use Case/User Flow definition, DB Schema & AWS Architecture design | Requirements spec, DB Schema, System Architecture Diagram |
| **Phase 2** | Weeks 3–5 | Auth/Author implementation, Transaction Management, Categories, Personal/Family Wallets, Budgets & Dashboard | Working core backend & frontend for transaction management |
| **Phase 3** | Weeks 6–8 | Azure Document Intelligence OCR integration, S3 image upload, SQS/DLQ pipeline, Gemini/OpenAI API & AI Insights | Automated receipt OCR & AI spending recommendation pipeline |
| **Phase 4** | Weeks 9–10 | Admin panel (Users, Tickets, Notifications, System Settings), Responsive Mobile UI optimization | Feature-complete Admin Portal & responsive UI |
| **Phase 5** | Week 11 | AWS Infrastructure deployment (VPC, Multi-AZ SQL Server, S3, SQS, ECS Fargate, ALB, Amplify, Route 53) & CI/CD | Full cloud-native platform live on AWS |
| **Phase 6** | Week 12 | End-to-end testing, OCR/AI evaluation, Security audit, SQS/DLQ resilience check, CloudWatch monitoring & docs | Demo-ready production release |

---

### 6. Budget Estimation

#### 6.1. Development & Demo Environment

| Service Component | Estimated Monthly Cost |
| :--- | :--- |
| AWS Amplify, CloudFront & Route 53 | $5 – $15 USD |
| Amazon S3 | $1 – $5 USD |
| ECS Fargate (Backend & AI Worker) | $20 – $50 USD |
| Application Load Balancer (ALB) | $18 – $25 USD |
| SQL Server (Dev Environment) | $30 – $80 USD |
| Amazon SQS, SNS & ECR | $2 – $10 USD |
| CloudWatch & AWS Budgets | $2 – $10 USD |
| AI Services (Azure Document Intelligence, Gemini, OpenAI) | Usage-based pay-as-you-go |
| **Total Estimated Cost** | **$80 – $200 USD / month** |

#### 6.2. Production Multi-AZ Environment

| Service Component | Estimated Monthly Cost |
| :--- | :--- |
| ECS Fargate & Application Load Balancer | $60 – $150 USD |
| SQL Server Primary/Standby (Multi-AZ) | $150 – $300 USD |
| Dual NAT Gateways & Data Transfer | $70 – $120 USD |
| S3, CloudFront, SQS, SNS, ECR & CloudWatch | $20 – $60 USD |
| External AI APIs | Usage-based on receipt volume |
| **Total Estimated Cost** | **$300 – $600 USD / month** *(excl. AI APIs)* |

> **Cost Optimization Strategy:** During development, single-AZ configs and limited container execution are used. Production environment seamlessly scales to Multi-AZ and Auto Scaling for High Availability.

---

### 7. Risk Assessment & Mitigation

| # | Risk Description | Severity | Mitigation & Contingency Strategy |
| :---: | :--- | :---: | :--- |
| **1** | OCR Extraction Errors (blurry, wrinkled, skewed receipts) | **High** | Allow user review & edit prior to saving; show side-by-side original image; highlight low-confidence fields. |
| **2** | External AI Service Dependency | **High** | Asynchronous SQS queueing; Exponential Backoff Retries; DLQ routing; decoupled AI Service layer for vendor switching; manual entry fallback. |
| **3** | Financial Data Leakage | **Critical** | Mandatory HTTPS encryption; Secrets stored in SSM Parameter Store; granular Admin/User RBAC; backend ownership checks; Audit Logging. |
| **4** | Unexpected AWS & AI Cost Overruns | **Med - High** | Configure AWS Budgets at 50%, 80%, 100% thresholds; compress images pre-upload; S3 Lifecycle Policies; AI request rate-limiting. |
| **5** | High Concurrency Receipt Scanning | **Medium** | Amazon SQS buffering; AI Worker Auto Scaling driven by SQS Queue Depth; strict decoupling of API Backend and AI Workers. |
| **6** | Deployment Regression Errors | **Medium** | Automated CI/CD pipelines; isolated dev/prod environments; ECS Rolling Updates; Automated Health Checks; ECR image rollback capability. |
| **7** | Inaccurate AI Financial Advice | **Medium** | Only evaluate user-confirmed transaction data; present AI Insights as advisory suggestions; gather user feedback to optimize prompts. |

---

### 8. Expected Outcomes

* **Complete SaaS Platform**: End-to-end operation from receipt capture, data extraction, secure storage, to analytics and financial alerting.
* **Superior User Experience**: Dramatically reduced manual entry overhead through smart OCR and interactive verification workflows.
* **Validated Cloud-Native AWS Architecture**: Demonstrated reliability of asynchronous queue processing (SQS), database redundancy (Multi-AZ DB), centralized monitoring (CloudWatch), and automated CI/CD.
* **Extensible Foundation**: Strong foundation for user feedback collection, continuous AI accuracy improvements, and future feature expansion.