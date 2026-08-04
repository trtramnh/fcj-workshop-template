---
title: "Cleanup"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---


The cleanup step is the most critical step for students and Cloud learners to avoid credit card charges. Please follow this sequentially from top to bottom to ensure resources do not lock each other out (e.g., a VPC cannot be deleted if the NAT Gateway is still alive).

> [!CAUTION]
> Strictly follow the order below. If you encounter a "Network Interface is in use" error, it means you haven't fully cleared the shared network resources.

### 1. Cleanup ECS Fargate
- Open **Amazon ECS ➔ Clusters ➔ `Snaptics-Cluster`**.
- Check `snaptics-backend-service` ➔ Click **Update**.
- Change `Desired tasks` to **0** ➔ Click **Update**. (This action forces AWS to shut down the servers).
- Go back to the Services tab, wait for the Tasks status to reach STOPPED. Check the Service again ➔ **Delete**.
- Delete the `Snaptics-Cluster` entirely.

### 2. Cleanup Load Balancer
- Open **EC2 ➔ Load Balancers**.
- Check `snaptics-alb` ➔ Select Actions ➔ **Delete**.
- Switch to the **Target Groups** tab, delete `snaptics-tg`.

### 3. Cleanup RDS Database
- Open **Amazon RDS ➔ Databases**.
- Check `snaptics-db` ➔ Actions ➔ **Delete**.
- **Important:** Uncheck "Create final snapshot", agree to delete automated backups, and type `delete me` into the confirmation box. Click **Delete**.

### 4. Cleanup NAT Gateway & Elastic IP (Most expensive)
- Open **VPC ➔ NAT Gateways**.
- Check `snaptics-nat-gw` ➔ **Delete NAT gateway**.
- Wait about 3 minutes for the word Deleted to appear.
- Open **VPC ➔ Elastic IPs**, check the IP you just created, Actions ➔ **Release Elastic IP addresses**. (Forgetting to release an Elastic IP will cause AWS to charge a $1/month idle fee).

### 5. Cleanup S3, ECR, SQS, SNS, Parameter Store
- **S3:** Go to the `s3-bucket-snaptics` bucket. You must click the **Empty** button (to clear files inside) before the system allows you to click the **Delete** bucket button.
- **ECR:** Go delete the repository containing your Docker image.
- **SQS/SNS:** Delete the `snaptics-main-queue` queue and `snaptics-alerts` topic.
- **Parameter Store:** Select each configuration row and click delete.

### 6. Delete VPC Last
- Open **VPC ➔ Your VPCs**.
- Check `snaptics-vpc` ➔ Actions ➔ **Delete VPC**.
- This action will automatically clean up all remaining Subnets, Route Tables, Internet Gateways, and Security Groups in a single click!

Congratulations, you have successfully completed the massive project of deploying the Snaptics API Multi-Stack Architecture on AWS!
