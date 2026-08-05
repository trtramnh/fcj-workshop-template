---
title: "System Architecture"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1.1. </b> "
---


<img src="/images/2-Proposal/snaptics_architecture.png" alt="Snaptics AWS Architecture" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />

The Snaptics Multi-Stack network design strictly adheres to Cloud-Native security principles: completely isolating Public Subnets and Private Subnets across 2 Availability Zones (AZs), orchestrating traffic via Amazon Route 53, CloudFront, Application Load Balancer (ALB), and automating CI/CD via GitHub Actions.



### Data Flow Analysis (Steps 1 to 10)

1. **[1] User Request:** Users access the system; **Amazon Route 53** resolves the domain to the **Amazon CloudFront** distribution endpoint.
2. **[2] Content Delivery & Edge Routing:** CloudFront serves the user interface (Frontend Web) hosted on **AWS Amplify**, while routing API requests through the **Internet Gateway**.
3. **[3] Ingress Load Balancing:** API traffic passes through the Internet Gateway to the **Application Load Balancer (ALB)** located in the Public Subnets.
4. **[4] Compute Dispatching:** ALB routes API requests to containerized backend tasks running on **ECS Cluster (Fargate Tasks)** safely isolated inside Private Subnets across 02 Availability Zones.
5. **[5] Object Storage Access:** Fargate Tasks read/write receipt images and static files directly to the **Amazon S3 Bucket** via a **Gateway Endpoint** within the VPC, bypassing the public Internet for optimal security and cost savings.
6. **[6] Database Persistence:** Fargate Tasks process business logic and persist data to **Aurora & RDS (Primary / Standby)** configured with Multi-AZ replication.
7. **[7] Asynchronous Queueing & DLQ:** Heavy processing tasks (such as OCR/AI extraction) are pushed to **Amazon SQS (`snaptics-ai-queue`)**. ECS Worker Tasks dequeue and process these jobs asynchronously. If a job fails beyond retry limits, it is routed to the **Dead Letter Queue (DLQ)** for inspection and reprocessing.
8. **[8] Outbound Egress:** When external API access is required, Fargate Tasks dispatch outbound requests via **NAT Gateways** in the Public Subnets.
9. **[9] Gateway Egress:** Traffic from NAT Gateways travels through the **Internet Gateway** out to the public Internet.
10. **[10] External AI Integration:** Requests reach **External AI APIs** (such as Azure Document Intelligence for invoice OCR and Google Gemini API for AI Insights). Extracted results are written back to Aurora & RDS.

---

### Auxiliary Components & CI/CD

* **Management & Observability:**
  * **Amazon CloudWatch:** Collects logs, metrics, and manages alarms for system operation.
  * **AWS Secrets Manager:** Securely stores and retrieves sensitive configurations (API Keys, JWT Secrets, Connection Strings).
  * **AWS Budgets:** Monitors infrastructure expenses and sends cost threshold alerts.
  * **Amazon SNS (Simple Notification Service):** Dispatches operational alerts and event notifications.

* **CI/CD Pipeline (GitHub Actions):**
  * Developers push code to the **GitHub Repo**, triggering **GitHub Actions**.
  * **Auto Build & Deploy:** Automatically builds and deploys Frontend assets to **AWS Amplify**.
  * **Build & Push Docker Images:** Builds Docker containers and pushes images to **Elastic Container Registry (ECR)**.
  * **Update Service:** Updates services on **ECS Cluster**, prompting Fargate Tasks to pull the latest image from ECR (**Pull Image**).

