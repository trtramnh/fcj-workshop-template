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
**Snaptics** is proposed to transform personal and family expense management from manual entry into a digitized, centralized, and intelligent analytical model. Users can snap or upload a photo of a receipt; the system automatically extracts key fields including merchant name, transaction date, total amount, category, and line-item details. Following user verification and confirmation, Snaptics automatically creates transaction records, updates wallet balances and budgets, and synchronizes data to the Dashboard.

Beyond receipt scanning, the system supports manual transaction logging, multi-wallet management (personal and shared family wallets), budget setting and utilization alerts, statistical reports, in-app notifications, AI Insights, and interactive AI Chatbot. The Admin Panel enables administrators to monitor users, handle support tickets, broadcast system announcements, configure runtime parameters, and manage Hangfire background jobs.

#### 1.2. User Roles
* **User (Personal / Family Member)**: Manages transactions, receipt images, wallets, budgets, and analytical reports; receives financial advice; initializes and participates in shared family wallets or budgets.
* **Admin (System Administrator)**: Manages user accounts, handles support tickets, issues system notifications, configures system parameters, monitors Hangfire background execution, and oversees cloud operations.

#### 1.3. Functional Scope
* Authentication, authorization, and role-based access control (User/Admin).
* Receipt image upload/scan, OCR result review, and pre-save editing.
* Transaction CRUD, automated/manual categorization, and history search.
* Personal wallet creation, shared family wallet management, and member access.
* Budget setting, utilization tracking, and threshold alerts.
* Dashboard, visual charts, and spending reports by time frame or category.
* AI Insight, AI Chatbot with conversation history retention.
* Centralized in-app notification system and technical support tickets.
* Periodic background jobs via Hangfire; asynchronous OCR/AI pipeline via Amazon SQS.
* Cloud deployment, monitoring, and cost control on AWS.

#### 1.4. Proposal Scope Limitations
The 13-week phase focuses on completing a responsive web application, automated receipt extraction, core expense/wallet/budget management, and end-to-end AWS cloud deployment under a cost-optimized demo configuration. Direct banking/e-wallet API integrations (MoMo, ZaloPay), native mobile apps (iOS/Android), legally binding financial advice, and continuous 24/7 Multi-AZ Production operations at scale are reserved for future development.

---

### 2. Project Goals

#### 2.1. General Objective
Build the Snaptics intelligent expense management platform powered by cloud computing and artificial intelligence, enabling users to minimize manual data entry time, proactively control budgets, and enhance personal and family financial analysis capabilities through visual data insights.

#### 2.2. Specific Objectives
* Automatically extract receipt data via OCR technology.
* Auto-create and categorize transactions upon user verification.
* Allow manual transaction entry and adjustments when receipts are missing or OCR results need editing.
* Manage multiple personal and family wallets/budgets with multi-member visibility.
* Provide interactive Dashboards and spending reports by day, week, month, and category.
* Analyze spending habits and present AI Insights as advisory recommendations.
* Trigger alerts when budget limits are approached or exceeded.
* Build a centralized notification center and AI Chatbot with chat history.
* Deliver an Admin Panel for managing users, support tickets, announcements, configurations, and background jobs.
* Deploy on AWS with scalability, security, and cost control (Amazon CloudWatch, AWS Budgets).
* Establish CI/CD pipelines to automate builds and deployments.

#### 2.3. Completion Criteria

| Criteria Category | Target Deliverable |
| :--- | :--- |
| **Business Flow** | End-to-end flow from receipt upload to transaction creation and Dashboard update. |
| **Financial Management** | Functional transactions, wallets, budgets, reports, notifications, and AI Insights within demo scope. |
| **Authorization** | Users access only their owned data; Admin accesses administrative functions based on granted roles. |
| **Cloud Deployment** | Frontend, AWS WAF, Backend, Worker, Database, Storage, Queue, Secrets, and Monitoring configured on AWS. |
| **Operations** | CloudWatch logging, health checks, cost alerts, and DLQ error message handling. |

---

### 3. Problem Statement

#### 3.1. Manual Expense Logging & Error Risks
Most users log expenses manually using notebooks, spreadsheets, or manual app entries, which is time-consuming, error-prone, and unsustainable. Snaptics automates receipt data capture via OCR while offering a review interface before persistence.

#### 3.2. Fragmented Financial Data
Expense data is scattered across paper receipts, banking apps, notes, and e-wallets. Snaptics consolidates all transactions into a single unified platform for clear cash flow visibility.

#### 3.3. Budget Control Challenges
Users often realize overspending only after budget limits are breached. Snaptics tracks budget utilization in real-time and triggers proactive alerts based on configured thresholds.

#### 3.4. Lack of Spending Behavior Insights
Raw transactions provide limited value without synthesis. Snaptics analyzes historical data across categories and time, detecting trends and providing actionable advice via AI Insight.

#### 3.5. Family Financial Coordination Issues
Shared household spending lacks visibility without centralized tracking. Snaptics provides shared family wallets and joint budget pools.

#### 3.6. OCR & AI Processing Latency
Synchronous OCR and AI processing can cause API timeouts under high load. Snaptics utilizes Amazon SQS and AI Workers to decouple heavy processing from Backend APIs.

#### 3.7. Automated Background Scheduling
Tasks like periodic AI Insights, budget checks, and alert pushes require automated scheduling. Hangfire is integrated into the .NET backend with an Admin interface for job management.

---

### 4. Solution Architecture

Snaptics utilizes an AWS Cloud-Native architecture combining a Single Page Application (SPA), edge protection layer, containerized microservices on ECS Fargate, relational database, object storage, asynchronous queues, and external AI services. The target infrastructure spans two Availability Zones, maintaining clear separation between Frontend, Backend API, and AI Workers for independent scaling, deployment, and monitoring.

#### 4.1. Design Principles
* Decouple OCR/AI processing from API requests to prevent timeouts and backend bottlenecks.
* Deploy VPC across two Availability Zones; place Backend, Worker, and Database in Private Subnets; expose only AWS Amplify, ALB, NAT Gateways, and necessary public endpoints.
* Store receipt images on Amazon S3 accessed internally via S3 Gateway Endpoint; store relational business data in Amazon RDS for SQL Server.
* Attach AWS WAF; store JWT secrets and connection strings securely in AWS Secrets Manager using least privilege.
* Monitor logs, metrics, queue errors, and costs starting from the demo phase.
* Clearly demarcate target production architecture from cost-optimized demo configurations.

#### 4.2. Solution Components

| Component | Role in System |
| :--- | :--- |
| **Frontend** | Angular Single Page Application automatically built and deployed via AWS Amplify from GitHub. AWS Amplify hosts and distributes the Frontend application. |
| **DNS & Domain Resolution** | Amazon Route 53 manages domain and DNS resolution. Frontend UI requests are routed to AWS Amplify, and API requests are sent via Internet Gateway to Application Load Balancer (ALB). |
| **Networking (VPC)** | Amazon VPC spanning 02 Availability Zones (AZs). Each AZ contains Public Subnets and Private Subnets. ALB and NAT Gateways reside in Public Subnets; ECS Fargate and Aurora & RDS databases reside in Private Subnets. |
| **Backend API (ECS Cluster)** | .NET API containerized with Docker, stored on Amazon ECR, and deployed as ECS Services running Fargate Tasks across Private Subnets behind ALB. |
| **AI Worker** | ECS Fargate Worker dequeuing messages from SQS `snaptics-ai-queue`, reading images from S3 via Gateway Endpoint, executing automated tasks via NAT Gateway, and writing results to Aurora & RDS. |
| **Database** | **Aurora & RDS (Primary / Standby)** managing accounts, transactions, wallets, budgets, notifications, chat history, and support tickets. Configured with Multi-AZ replication between Primary (AZ 2) and Standby (AZ 1). |
| **Receipt Storage** | Amazon S3 stores raw receipt images and processed files. Backend/Worker access S3 internally via **S3 Gateway Endpoint** within the VPC, reducing NAT Gateway transfer costs. |
| **Asynchronous Queue** | **Amazon SQS (`snaptics-ai-queue`)** buffers OCR/AI tasks; **Dead Letter Queue (DLQ)** captures failed messages exceeding retry limits for inspection and replay. |
| **Secrets & Security** | **AWS Secrets Manager** encrypts and stores RDS connection strings and JWT secrets. IAM Roles and Security Groups enforce least privilege. |
| **Management & Observability** | **Amazon CloudWatch** (logs/metrics/alarms), **AWS Secrets Manager** (sensitive config), **AWS Budgets** (cost tracking), and **Simple Notification Service (SNS)** (operational alerts). |
| **CI/CD Pipeline** | GitHub Actions triggers 3 automated paths: (1) **Auto Build & Deploy** Frontend to AWS Amplify, (2) **Build & Push Docker Images** to Elastic Container Registry (ECR), and (3) **Update Service** on ECS Cluster for Fargate to pull new images (**Pull Image**). |

#### 4.3. Main Workflow Steps (Matching Architecture Diagram)
1. User accesses the Snaptics domain; **Amazon Route 53** (1) resolves DNS routing Frontend UI requests to **AWS Amplify** and API requests to the system.
2. **AWS Amplify** distributes the Frontend SPA UI. API request traffic passes through the **Internet Gateway** (2) into the **Application Load Balancer (ALB)**. **AWS WAF** inspects and blocks security threats.
3. API requests pass through the Internet Gateway to the **Application Load Balancer (ALB)** (3) in Public Subnets.
4. **ALB** forwards API traffic to **ECS Fargate Tasks** (4) running in Private Subnets across 02 Availability Zones.
5. Fargate Tasks read/write receipt images directly to **Amazon S3** via **S3 Gateway Endpoint** (5) in the VPC, bypassing the public Internet.
6. Fargate Tasks read/write relational business data on **Aurora & RDS Primary / Standby** (6) synchronized across Multi-AZ.
7. Fargate Tasks push OCR/AI jobs to **Amazon SQS (`snaptics-ai-queue`)** (7); AI Worker processes messages, sending failed tasks to **Dead Letter Queue (DLQ)**. ECS Fargate Tasks pull Docker images from **Amazon ECR**.
8. Fargate Workers in Private Subnets dispatch outbound requests (8) to external AI services.
9. Outbound traffic routes through **NAT Gateways** (9) in Public Subnets connected to the Internet Gateway.
10. Outbound traffic reaches external services (10); credentials are pulled securely from **AWS Secrets Manager**.

#### 4.4. Overall Architecture Diagram

![Snaptics AWS Cloud Architecture](/images/2-Proposal/snaptics_architecture.png)
*Figure 1. Target AWS Cloud Architecture for Snaptics System*

#### 4.5. Security, Observability & Cost Control
* Enforce HTTPS and access tokens; attach AWS WAF for edge filtering.
* Enforce RBAC and data ownership checks at the Backend layer.
* Zero hardcoded secrets in source code or Docker Images; store JWT secrets and connection strings in AWS Secrets Manager.
* Place Backend, AI Worker, and RDS SQL Server in Private Subnets; limit Public Subnet access strictly to ALB and NAT Gateways with tight Security Groups.
* Access S3 internally via S3 Gateway Endpoint; validate file types, size caps, and image formats prior to processing.
* Aggregate CloudWatch Logs for system errors, AI worker tasks, Admin actions, Hangfire status, and SQS/DLQ queues.
* Configure CloudWatch Alarms for ALB/ECS/RDS, monitor SQS Queue Depth and DLQ; dispatch alerts via Amazon SNS and AWS Budgets.
* Enforce ECS Auto Scaling boundaries, NAT/RDS/Fargate runtime limits, and prompt demo resource cleanup.

---

### 5. Project Timeline (13 Weeks)

| Time | Focus | Core Tasks | Deliverables |
| :--- | :--- | :--- | :--- |
| **Week 1** | Research & Cloud Strategy | Project requirements survey, expense tracking challenges, SaaS model & AWS cloud service selection. | Scope definition & cloud research direction. |
| **Week 2** | Requirements & Preliminary Design | Analyze receipt scanning, transactions, wallets, budgets; research OCR/AI; select Angular, .NET, SQL Server & AWS architecture. | Requirements spec & initial architecture diagram. |
| **Week 3** | Snaptics Concept Formation | Finalize project name, User/Admin roles, core features & demo scope; build product backlog. | Finalized concept & product backlog. |
| **Week 4** | Source Code Initialization | Create GitHub Repo; initialize Angular Frontend & .NET Backend; establish repository structure and branching strategy. | Repository & project structure ready for development. |
| **Week 5** | Transactions & Categories | Develop APIs and UI for transaction CRUD, search, and category management. | Working core transaction & category system. |
| **Week 6** | Wallets & Budgets | Develop personal/family wallets, shared member access, budgets, and utilization logic. | Complete wallet & budget management system. |
| **Week 7** | Dashboard & S3 Storage | Build Dashboard, charts, spending reports; receipt upload UI; integrate Amazon S3 storage. | Interactive Dashboard & S3 image storage. |
| **Week 8** | OCR, AI & Notifications | Build automated invoice processing module; result normalization; AI Insights & in-app notifications. | Automated OCR & complete AI advice pipeline. |
| **Week 9** | DLQ & Hangfire | Create SQS Dead Letter Queue & async AI Worker; integrate HangfireController & Admin job scheduler UI. | Async AI tasks via SQS/DLQ & Hangfire Admin. |
| **Week 10** | AWS Frontend, Secrets & Database | Connect AWS Amplify to GitHub; deploy Frontend; store JWT secrets & DB string in Secrets Manager; provision demo RDS. | Live Frontend on Amplify & secure RDS SQL Server. |
| **Week 11** | VPC, S3 Endpoint, SQS & Containers | Provision VPC (2 AZs), Subnets, Internet Gateway, NAT Gateway, S3 Gateway Endpoint & SQS/DLQ; containerize Backend/Worker; ECR & GitHub Actions pipeline. | Network infrastructure, S3 endpoint & ECR container pipeline. |
| **Week 12** | ECS Fargate Deployment | Create ECS Cluster/Service; deploy Backend & AI Worker; configure ALB, health checks, Auto Scaling, Secrets Manager access, CloudWatch & Budgets. | Backend & Worker running on ECS Fargate. |
| **Week 13** | Polishing, Testing & Demo | Configure Route 53 & AWS WAF; responsive UI testing, RBAC, S3 Endpoint, SQS-Worker-DLQ, RDS, logs, CI/CD & cost audit. | Demo-ready Snaptics platform. |

---

### 6. Budget Estimation

The budget estimation covers a 1-month development, integration, and demo window. This represents a cost-optimized demo configuration; actual expenses depend on resource uptime, traffic, OCR page volume, AI token counts, and cloud vendor pricing. The conversion rate used for planning is 1 USD = 26,000 VND.

#### 6.1. Budget Estimation Assumptions

| Parameter | Assumption |
| :--- | :--- |
| **Test Users** | ~100 active test users |
| **Receipt Volume** | 1,000 receipts / OCR pages during demo month |
| **S3 Storage** | ~20 GB receipt images and processed files |
| **Frontend Traffic** | ~30-50 GB / month |
| **Backend & AI Worker** | Small task configs; total ~200-220 task-hours during integration/demo |
| **Database** | RDS for SQL Server Express, Single-AZ, ~20 GB |
| **Outbound Internet Access** | 01 NAT Gateway, maintained strictly during required integration windows |
| **Exclusions** | Domain purchase fees, taxes, continuous 24/7 WAF, and Multi-AZ production operation |

#### 6.2. Demo Environment Estimated Budget

| # | Service Component | Estimation Basis | Cost (USD) |
| :---: | :--- | :--- | :---: |
| **1** | AWS Amplify & Route 53 | Build/hosting Frontend, low traffic distribution and 01 Hosted Zone | $4.50 |
| **2** | Amazon S3 | Store ~20 GB receipt images and upload/download requests | $1.00 |
| **3** | ECS Fargate - Backend & AI Worker | Small task configs, total ~200-220 task-hours | $8.00 |
| **4** | Application Load Balancer (ALB) | Active during deployment & demo phase, low traffic | $7.00 |
| **5** | Amazon RDS for SQL Server | SQL Server Express, Single-AZ, 20 GB | $20.00 |
| **6** | NAT Gateway & Data Transfer | 01 NAT Gateway, enabled limited time during integration | $13.00 |
| **7** | Amazon SQS, SNS & ECR | Queue OCR/AI, basic alerting, and Docker Image storage | $1.00 |
| **8** | CloudWatch, Secrets Manager & AWS Budgets | Logs, metrics, alarms, secrets/API keys storage & budget alerts | $3.00 |

#### 6.3. Cost Control Measures
* Create AWS Budgets and set alert triggers at 50%, 80%, and 100% threshold levels.
* Maintain NAT Gateway, ALB, RDS, Fargate, and AWS WAF only during active integration and demo windows.
* Set min/max bounds for ECS Auto Scaling to prevent idle AI Worker tasks.
* Enforce client-side image compression and file size limits prior to upload.
* Configure CloudWatch log retention policies and S3 Lifecycle Policies.
* Prevent duplicate processing calls when valid transaction data exists; never write secrets to logs.
* Clean up all demo resources immediately upon completion.

#### 6.4. Pricing References
Calculations use pay-as-you-go public rates. Figures in the table represent internal planning targets for the demo; exact pricing should be confirmed using the AWS Pricing Calculator before provisioning.

* [AWS Amplify Pricing](https://aws.amazon.com/amplify/pricing/)
* [AWS Fargate Pricing](https://aws.amazon.com/fargate/pricing/)
* [Amazon Route 53 Pricing](https://aws.amazon.com/route53/pricing/)
* [Amazon VPC / NAT Gateway Pricing](https://aws.amazon.com/vpc/pricing/)
* [Elastic Load Balancing Pricing](https://aws.amazon.com/elasticloadbalancing/pricing/)
* [AWS WAF Pricing](https://aws.amazon.com/waf/pricing/)
* [AWS Secrets Manager Pricing](https://aws.amazon.com/secrets-manager/pricing/)
* [Amazon RDS for SQL Server Pricing](https://aws.amazon.com/rds/sqlserver/pricing/)

---

### 7. Risk Assessment & Mitigation

| # | Risk Description | Impact | Probability | Severity | Mitigation & Contingency Strategy |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **1** | OCR Extraction Errors (blurry, wrinkled, skewed receipts) | High | Medium | **High** | Allow user review & edit before saving; display original image side-by-side; highlight low-confidence fields. |
| **2** | Automated Task Service Failure / Rate Limit | High | Medium | **High** | Asynchronous SQS processing; Exponential Backoff Retries; DLQ routing; decoupled AI Service layer; manual entry fallback. |
| **3** | Financial Data Leakage or API Key Exposure | Critical | Low | **High** | Mandatory HTTPS; AWS WAF; Secrets Manager for JWT secrets, and DB connections; IAM least privilege; Private Subnets; zero logging of secrets. |
| **4** | Unexpected AWS & AI Cost Overruns | High | Medium | **High** | AWS Budgets alerts at 50%, 80%, 100%; Auto Scaling caps; pre-upload image compression; S3 Lifecycle Policy; post-demo resource cleanup. |
| **5** | High Concurrency Receipt Scanning Queue Backlog | Medium | Medium | **Medium** | Monitor SQS Queue Depth; scale AI Workers within limits; display processing status UI; decouple Backend API from Workers. |
| **6** | Deployment Regression Errors | Medium | Medium | **Medium** | Automated CI/CD build tests; ECS Health Checks; stable Docker Tags on ECR; Dev/Prod environment isolation; CloudWatch monitoring. |
| **7** | Generic or Irrelevant AI Advice | Medium | Medium | **Medium** | Analyze only user-confirmed transactions; present AI Insights as advisory suggestions; collect user feedback to refine Prompts. |
| **8** | Hangfire Misconfigurations or Failed Jobs | Medium | Medium | **Medium** | Verify timezone configs; Admin-only job management; limit concurrent executions; detailed logging & manual trigger capability. |
| **9** | Scope Creep Beyond 13-Week Timeline | High | Medium | **High** | Prioritize core workflow; lock demo scope; split backlog into required vs optional; test early and defer non-essential features. |

---

### 8. Conclusion & Expected Outcomes

Snaptics aims to transform expense management from manual tracking to an automated, centralized, and analytical platform. Combining Amazon SQS/DLQ, Hangfire, ECS Fargate, RDS SQL Server, S3 Gateway Endpoint, AWS WAF, and Secrets Manager ensures secure, efficient receipt processing while laying the foundation for future financial capabilities.

Upon completing the 13-week project, the team will deliver a fully functional demo showcasing end-to-end receipt scanning, transaction management, wallet/budget sync, visual reports, and in-app notifications, while demonstrating cloud-native deployment, containerization, queueing, security, monitoring, and CI/CD automation on AWS.

| Deliverable Category | Target Outcome |
| :--- | :--- |
| **Product** | Functional Web application covering User/Admin roles and core business workflows. |
| **Technical** | Demonstrated AWS WAF, containerized ECS Fargate, async queueing, S3 Gateway Endpoint, RDS SQL Server, and External AI API integration. |
| **Operations** | Health checks, CloudWatch logs/metrics, DLQ fault handling, Secrets Manager, cost alerts, and scheduled background job management. |
| **Scalability** | Architecture ready to transition smoothly from cost-optimized demo to Multi-AZ production deployment. |