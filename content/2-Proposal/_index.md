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

#### 1.1. Introduction
**Snaptics** is a personal and family financial management platform proposed to transform spending tracking from manual entry into an automated, centralized, and intelligent analysis model. Users can snap a photo or upload an image of a receipt; Optical Character Recognition (OCR) and Artificial Intelligence (AI) technologies automatically extract key information including merchant name, transaction date, total amount, category, and line-item details. After user review and confirmation, Snaptics automatically creates transaction records, updates wallet balances, synchronizes budgets, and reflects data on the financial dashboard.

Beyond automated receipt scanning, Snaptics provides comprehensive features: manual transaction logging, multi-wallet management (personal & shared family wallets), budget setting and threshold alerts, multi-dimensional statistical reporting, a centralized notification hub, spending behavioral analysis (AI Insight), and an interactive smart assistant (AI Chatbot). The system includes an Admin Panel enabling administrators to monitor users, handle support tickets, issue system announcements, configure runtime settings, and manage background jobs via Hangfire.

Snaptics is built as a SaaS web application deployed on AWS Cloud-Native infrastructure. The system architecture leverages services including **AWS Amplify**, **Amazon CloudFront**, **Amazon Route 53**, **Amazon ECS Fargate**, **Application Load Balancer (ALB)**, **Amazon SQS / Dead Letter Queue (DLQ)**, **Amazon S3**, **Amazon ECR**, **AWS Systems Manager Parameter Store**, **Amazon CloudWatch**, **AWS Budgets**, and **Amazon RDS for SQL Server**. AI capabilities are integrated with **Azure Document Intelligence**, **Gemini API**, and **OpenAI API**.

#### 1.2. User Roles
* **User (Personal / Family Member)**: Manages transactions, receipt images, wallets, and budgets; views analytical reports; receives AI financial advice; initializes and participates in shared family wallets/budgets.
* **Admin (System Administrator)**: Manages user accounts, handles support tickets, broadcasts system notifications, configures system parameters, monitors Hangfire background schedules, and oversees platform health.

#### 1.3. Functional Scope
* **Authentication & Authorization**: User login, token authentication (JWT/OAuth2), and role-based access control (User/Admin).
* **Receipt Processing Pipeline**: Image upload, automated OCR extraction, user verification and editing before saving.
* **Transaction Management**: Creation, updates, automatic/manual categorization, and transaction history searching.
* **Financial Wallet Management**: Personal wallet creation, shared family wallet management, and member access management.
* **Budget Management**: Threshold setting, real-time usage calculation, and automatic threshold alerts.
* **Analytics & Reports**: Visual dashboard displaying spending charts by time frame and category.
* **AI Support Features**: Spending habit analysis (AI Insight), interactive conversational assistant (AI Chatbot) with conversation history.
* **Notifications & Support**: Centralized in-app notifications and technical support ticket management.
* **System Jobs & Background Execution**: Scheduled background tasks via Hangfire; asynchronous OCR/AI pipeline via Amazon SQS and Dead Letter Queue (DLQ).
* **Cloud Operation**: AWS deployment, centralized performance monitoring (Amazon CloudWatch), and cost management (AWS Budgets).

#### 1.4. Proposal Scope Limitations
The 13-week phase focuses on completing a responsive web application, automated receipt extraction, core expense/wallet/budget management, and end-to-end AWS cloud deployment under a cost-optimized demo configuration.

Features reserved for future development orientation include:
* Direct integration with Open Banking APIs or e-wallets (MoMo, ZaloPay).
* Native mobile applications (iOS / Android).
* Legally binding financial advisory recommendations.
* Continuous 24/7 Multi-AZ Production operation at scale.

---

### 2. Project Goals

#### 2.1. General Objective
Build the Snaptics intelligent expense management platform powered by cloud computing and artificial intelligence, enabling users to minimize manual data entry time, proactively control budgets, and enhance personal and family financial analysis capabilities through visual data insights.

#### 2.2. Specific Objectives
* **Automation**: Automatically extract receipt data via OCR (Azure Document Intelligence); auto-create and categorize transactions upon user verification.
* **Flexible Management**: Support manual transaction entry; manage multiple personal and family wallets/budgets; allow multi-member collaboration on shared financial resources.
* **Analytics & Reporting**: Provide interactive dashboards and spending reports by day, week, month, and category; analyze spending habits with AI Insights; offer an interactive AI Chatbot with history.
* **Alerts & Notifications**: Proactively monitor budget usage, sending alerts when limits are approached or exceeded; build a centralized notification center.
* **System Administration**: Deliver an Admin Panel for managing users, support tickets, system announcements, configurations, and Hangfire background jobs.
* **Cloud Operation & Deployment**: Deploy on AWS with scalability, security, and centralized monitoring (CloudWatch, AWS Budgets); establish CI/CD automation for testing and delivery.

---

### 3. Problem Statement

* **Manual Expense Entry & Data Error Risks**: Most users log expenses manually using notebooks, spreadsheets, or manual app entries, which is time-consuming, error-prone, and unsustainable. Snaptics automates receipt data capture via OCR while offering a review interface before persistence.
* **Fragmented Financial Data**: Expense information is scattered across paper receipts, banking apps, and e-wallets. Snaptics consolidates all transactions into a single unified platform.
* **Budget Control Challenges**: Users often realize overspending only after budget limits are breached. Snaptics tracks budget utilization in real-time and triggers proactive alerts.
* **Lack of Spending Behavior Insights**: Raw transactions provide limited value without synthesis. Snaptics analyzes historical data across categories and time, detecting trends and providing actionable advice via AI Insight.
* **Family Financial Coordination Issues**: Shared household spending lacks visibility without centralized tracking. Snaptics provides shared family wallets and joint budget pools.
* **AI Service Latency & Bottleneck Risks**: Synchronous OCR and AI processing can cause API timeouts under high load. Snaptics utilizes Amazon SQS and AI Workers to decouple heavy processing from Backend APIs.
* **Automated Background Job Scheduling**: Administrative tasks like periodic AI Insights generation, budget checks, and alert pushes require automated scheduling. Hangfire is integrated into the .NET backend with an Admin interface for job management.

---

### 4. Solution Architecture

Snaptics utilizes an AWS Cloud-Native architecture combining a Single Page Application (SPA), containerized microservices, relational databases, object storage, asynchronous queues, and specialized external AI services. The architecture maintains clear separation between Frontend, Backend API, and AI Workers for independent deployment, scaling, and monitoring.

![Snaptics AWS Cloud Architecture](/images/2-Proposal/snaptics_architecture.jpg)

#### 4.1. Key Components

* **Frontend**: Single Page Application (SPA) deployed via **AWS Amplify**. Connected to GitHub Repository for automated builds/deploys, integrated with **Amazon Route 53** for DNS and **Amazon CloudFront** (CDN) for secure HTTPS delivery.
* **Backend API**: Containerized via Docker stored on **Amazon ECR**, running on **AWS Fargate (Amazon ECS Cluster)** behind an **Application Load Balancer (ALB)** with **Auto Scaling**.
* **AI Worker**: Worker running on ECS Fargate dequeuing messages from Amazon SQS, invoking **Azure Document Intelligence** for OCR, and sending structured data to **Gemini API** / **OpenAI API** for classification and insights.
* **Database**: **Amazon RDS for SQL Server** deployed in Private Subnets, managing accounts, transactions, receipts, wallets, budgets, notifications, AI chat history, tickets, and audit logs.
* **Media Storage**: **Amazon S3** stores raw receipt images, transaction attachments, and processed images, decoupling binary assets from database storage.
* **Asynchronous Queue Pipeline**: **Amazon SQS** buffers OCR/AI tasks; **Dead Letter Queue (DLQ)** holds failed messages exceeding retry limits for fault handling.
* **Background Jobs & Scheduling**: **Hangfire** runner running alongside the .NET Backend to schedule, trigger, and monitor periodic system tasks.
* **Notification System**: Managed centrally in DB combined with **Amazon SNS** for pushing alerts (budget warnings, OCR status, AI tips, system updates).
* **Security & Configuration**: Secrets stored in **AWS Systems Manager Parameter Store**, Private Subnets for Backend/Worker/DB, mandatory HTTPS, Access Token (JWT) auth, RBAC, and Audit Logging.
* **CI/CD Pipeline**: Automated via **GitHub Actions** (Docker build, ECR push, ECS service update) and **AWS Amplify** (frontend builds).
* **Monitoring & Cost Control**: **Amazon CloudWatch** (logs, container metrics, SQS depth) and **AWS Budgets** (cost alerts at configured thresholds).

#### 4.2. Receipt Processing Flow
1. User uploads a receipt image via the Frontend.
2. Frontend sends image to Backend API -> Backend saves image to **Amazon S3**.
3. Backend pushes a processing message to **Amazon SQS**.
4. **AI Worker** retrieves message from SQS, calls **Azure Document Intelligence** for text/table extraction.
5. AI Worker sends extracted data to **Gemini API / OpenAI API** for category normalization and transaction parsing.
6. AI Worker saves transaction records into **Amazon RDS for SQL Server** and generates user notifications.
7. Frontend Dashboard and Financial Reports automatically update.

---

### 5. Project Timeline (13 Weeks)

| Phase | Duration | Focus & Core Tasks | Expected Outcomes |
| :--- | :--- | :--- | :--- |
| **Phase 1** | Week 1 | **Topic & AWS Cloud Research**: Research project requirements, expense tracking challenges, SaaS model & AWS services. | Cloud direction & research scope |
| **Phase 2** | Week 2 | **Requirements Survey & Preliminary Design**: Analyze receipt scanning, wallets, budgets; research OCR/AI; select Angular, .NET, SQL Server & AWS. | Requirements spec & Architecture diagram |
| **Phase 3** | Week 3 | **Snaptics Idea Formation**: Finalize project name, User/Admin roles, core features & demo scope; build product backlog. | Finalized concept & demo scope |
| **Phase 4** | Week 4 | **Source Code Initialization**: Create GitHub Repository; initialize Angular Frontend & .NET Backend; organize code structure. | Repository structure ready for development |
| **Phase 5** | Week 5 | **Transactions & Categories**: Develop APIs and UI for CRUD operations, searching transactions; category management. | Working core transaction system |
| **Phase 6** | Week 6 | **Wallets & Budgets**: Develop personal/family wallets, shared members, budgets, and utilization logic. | Complete family wallet & budget system |
| **Phase 7** | Week 7 | **Dashboard & S3 Storage**: Build Dashboard, charts, reports; develop receipt scanning UI; integrate Amazon S3 storage. | Interactive Dashboard & S3 integration |
| **Phase 8** | Week 8 | **OCR, AI & Notifications**: Integrate Azure Document Intelligence; normalize results; integrate Gemini API; build AI Insights & Chatbot. | Automated OCR & complete AI advice pipeline |
| **Phase 9** | Week 9 | **DLQ & Hangfire**: Create SQS Dead Letter Queue & async AI Worker; integrate HangfireController & Admin scheduler UI. | Async AI tasks via SQS/DLQ & Hangfire Admin |
| **Phase 10** | Week 10 | **AWS Frontend & Database**: Connect AWS Amplify to GitHub; store secrets in Parameter Store; initialize demo RDS SQL Server. | Frontend live on Amplify & RDS SQL Server |
| **Phase 11** | Week 11 | **VPC, SQS, ECR & Containerization**: Create VPC, SQS, Private Subnets; containerize Backend/Worker; ECR & GitHub Actions pipeline. | Private network & ECR container pipeline |
| **Phase 12** | Week 12 | **ECS Fargate Deployment**: Create ECS Cluster/Service; deploy Backend & AI Worker; configure ALB, Auto Scaling, CloudWatch & Budgets. | Backend/Worker running smoothly on Fargate |
| **Phase 13** | Week 13 | **Polishing, Testing & Demo**: Configure Route 53/CloudFront; end-to-end testing, responsive UI, RBAC, SQS/DLQ resilience; audit logs & costs. | Demo-ready Snaptics platform |

---

### 6. Budget Estimation

#### 6.1. Demo Environment Estimated Budget (1 Month Development & Demo)

| # | Service Component | Estimation Basis | Cost (USD) |
| :---: | :--- | :--- | :---: |
| **1** | AWS Amplify, CloudFront & Route 53 | Build/hosting Frontend, low traffic CDN and 01 Hosted Zone | $4.50 |
| **2** | Amazon S3 | Store ~20 GB receipt images and upload/download requests | $1.00 |
| **3** | ECS Fargate - Backend & AI Worker | Small task configs, total ~200-220 task-hours | $8.00 |
| **4** | Application Load Balancer (ALB) | Active during deployment & demo phase, low traffic | $7.00 |
| **5** | Amazon RDS for SQL Server | SQL Server Express, Single-AZ, 20 GB | $20.00 |
| **6** | NAT Gateway & Data Transfer | 01 NAT Gateway, enabled limited time during integration | $13.00 |
| **7** | Amazon SQS, SNS & ECR | OCR/AI Queue, basic alerting, and Docker Image storage | $1.00 |
| **8** | CloudWatch, Parameter Store & Budgets | Logs, metrics, alarms, secrets, and budget alerts | $3.00 |
| **9** | Azure Document Intelligence | ~1,000 pages using prebuilt invoice model | $10.00 |
| **10** | Gemini API | Estimated 1M input tokens and 200k output tokens | $0.80 |

#### 6.2. Production Multi-AZ Environment (Future Scaling Reference)

| Service Component | Estimated Monthly Cost |
| :--- | :--- |
| ECS Fargate & Application Load Balancer (Auto Scaling) | $60 – $150 USD |
| SQL Server Primary/Standby (Multi-AZ) | $150 – $300 USD |
| Dual NAT Gateways & Data Transfer | $70 – $120 USD |
| S3, CloudFront, SQS, SNS, ECR & CloudWatch | $20 – $60 USD |
| External AI APIs (Azure Document Intelligence & Gemini) | Usage-based on actual receipt volume |
| **Total Estimated Production Budget** | **$300 – $600 USD / month** *(excl. AI APIs)* |

---

### 7. Risk Assessment & Mitigation

| # | Risk Description | Impact | Probability | Severity | Mitigation & Contingency Strategy |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **1** | OCR Extraction Errors (blurry, wrinkled, skewed receipts) | High | Medium | **High** | Allow user review & edit before saving; display original image side-by-side; highlight low-confidence fields. |
| **2** | Azure Document Intelligence or Gemini API Failure / Rate Limit | High | Medium | **High** | Asynchronous SQS processing; Exponential Backoff Retries; DLQ routing; decoupled AI Service layer; manual entry fallback. |
| **3** | Financial Data Leakage or API Key Exposure | Critical | Low | **High** | Mandatory HTTPS; Secrets in SSM Parameter Store; Least-privilege IAM; Private Subnets for DB/Worker; Audit Logging. |
| **4** | Unexpected AWS & AI Cost Overruns | High | Medium | **High** | AWS Budgets alerts at 50%, 80%, 100%; Auto Scaling caps; pre-upload image compression; S3 Lifecycle Policy; cleanup demo resources post-use. |
| **5** | High Concurrency Receipt Scanning Queue Backlog | Medium | Medium | **Medium** | Monitor SQS Queue Depth; scale AI Workers within limits; display processing status UI; decouple Backend API from Workers. |
| **6** | Deployment Regression Errors | Medium | Medium | **Medium** | Automated CI/CD build tests; ECS Health Checks; stable Docker Tags on ECR; Dev/Prod environment isolation; CloudWatch monitoring. |
| **7** | Generic or Irrelevant AI Advice | Medium | Medium | **Medium** | Analyze only user-confirmed transactions; present AI Insights as advisory suggestions; collect user feedback to refine Prompts. |
| **8** | Hangfire Misconfigurations or Failed Jobs | Medium | Medium | **Medium** | Verify timezone configs; Admin-only job management; limit concurrent executions; detailed logging & manual trigger capability. |
| **9** | Scope Creep Beyond 13-Week Timeline | High | Medium | **High** | Prioritize core workflow; lock demo scope; split backlog into required vs optional; test early and defer non-essential features. |

---

### 8. Expected Outcomes

* **Complete Web SaaS Solution**: Seamless end-to-end operation from receipt scanning, OCR extraction, personal & family budget management, to analytics reports and financial alerts.
* **Demonstrated Cloud-Native AWS Architecture**: Proven container packaging (ECR, ECS Fargate), asynchronous queues (SQS, DLQ), relational database management (RDS SQL Server), security (SSM Parameter Store), centralized monitoring (CloudWatch, Budgets), and CI/CD automation.
* **Stable Operations & Cost Management**: Health checks, comprehensive logging, DLQ fault handling, and cost-controlled demo model ($76.92 USD budget).
* **Extensible Architecture**: Built to seamlessly scale from Single-AZ demo to Multi-AZ Production and support future enhancements (Open Banking, Native Mobile App).