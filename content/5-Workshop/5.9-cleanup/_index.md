---
title: "Cleanup"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---


Because this is a massive Enterprise-grade architecture, keeping it running 24/7 will incur significant AWS charges (especially SQL Server Multi-AZ, WAF, and NAT Gateway). You MUST tear it down immediately after testing.

Follow this exact reverse order to prevent dependency lock errors:

### 1. Frontend & DNS
- **AWS Amplify:** Go to the Amplify console, select the Snaptics app, click Actions ➔ **Delete app**.

- **AWS WAF:** Go to WAF, Web ACLs, select `snaptics-waf-acl` and **Delete**.
- **Route 53 (Optional):** Delete any custom records you created. Do not delete the Hosted Zone if you paid for the domain.

### 2. Compute Layer (ECS & ALB)
- **ECS Fargate:** Go to `Snaptics-Cluster`, update the service desired tasks to `0`. Wait for them to stop. Delete the Service. Then, Delete the Cluster.
- **Load Balancer:** Go to EC2 ➔ Load Balancers. Delete `snaptics-alb`.
- **Target Groups:** Delete `snaptics-ecs-tg`.

### 3. Data & Storage Layer
- **Amazon RDS for SQL Server:** Go to RDS ➔ Databases. Select the `snaptics-sql-server`. Click Actions ➔ **Delete**. *Crucial: Uncheck "Create final snapshot", acknowledge the warnings, and type `delete me`.*
- **AWS Systems Manager Parameter Store:** Select `/snaptics/prod/db-connection` ➔ Actions ➔ **Schedule secret deletion** (set to 7 days).
- **Amazon S3:** Go to your bucket. Click **Empty** to permanently delete all uploaded invoices. Then click **Delete** to remove the bucket itself.
- **Amazon ECR:** Delete the `snaptics-api` repository containing your Docker images.

### 4. Networking & VPC
- **NAT Gateway:** Go to VPC ➔ NAT Gateways. Delete `snaptics-nat-gw`.
- **Elastic IP (Crucial):** Wait 3 minutes for the NAT Gateway to disappear. Then go to **Elastic IPs**, select the IP, and **Release** it. If you forget this, AWS charges an idle fee!
- **VPC Endpoints:** Go to VPC ➔ Endpoints. Delete the S3 Gateway Endpoint.
- **VPC:** Finally, go to VPC ➔ Your VPCs. Select `snaptics-vpc` and click **Delete VPC**. This will elegantly wipe out all remaining subnets, route tables, and security groups.

### 5. Identity (IAM & GitHub)
- **IAM:** Delete the `github-actions-snaptics` user and the two ECS Roles.
- **GitHub Secrets:** Remove the AWS credentials from your GitHub repository settings to keep your account safe.

Congratulations on successfully deploying (and safely destroying) a real-world AWS Enterprise Architecture!