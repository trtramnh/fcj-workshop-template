---
title: "Cost Estimation"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.1.3. </b> "
---


Understanding the AWS billing structure is a vital skill for a Cloud Engineer. Snaptics' architecture includes multiple Managed Services that can incur significant monthly costs if not optimized.

Below is an estimated monthly cost breakdown for a **Basic Production** environment in the `ap-southeast-1` (Singapore) region, assuming the system serves around 10,000 requests per month.

| AWS Service | Configuration | Estimated Cost (USD/mo) | Note |
| :--- | :--- | :--- | :--- |
| **Amazon VPC (NAT Gateway)** | 1 NAT Gateway, 10 GB Data Processed | **~$35.00** | NAT Gateway charges an hourly rate ($0.059/hr), making it the heaviest fixed cost. |
| **Amazon RDS (SQL Server)** | `db.t3.micro`, Single-AZ, 20GB Storage | **~$22.00** | Express Edition. If using Multi-AZ, the cost doubles (~$44). |
| **Application Load Balancer** | 1 ALB running continuously for 730 hours | **~$18.00** | Additional fees apply based on connections (LCU). |
| **Amazon ECS Fargate** | 1 Task (0.25 vCPU, 1 GB RAM) running continuously | **~$10.00** | Cost is based on allocated RAM and CPU. Spot Instances can reduce this by 70%. |
| **Amazon S3** | 50 GB Storage, 10,000 requests | **~$1.50** | Very cheap, billed by actual GB stored. |
| **Amazon SQS / SNS** | Under 1 million requests | **~$0.00** | AWS Free Tier provides 1 million free messages per month. |
| **Parameter Store (Standard)** | Storing < 100 Secrets | **~$0.00** | Free. (Advanced tier incurs charges). |
| **Total (Estimated)** | | **~$86.50** | |

> [!WARNING]
> The above prices are **Estimates**. In a real environment, if you leave the system running and forget to turn it off (especially NAT Gateway and RDS), your AWS account will be charged around $80 - $90 at the end of the month. Always remember to perform the **Cleanup** section at the end of the Workshop!

### External AI APIs

Costs for AI are not billed by AWS but paid to Google and Microsoft:
- **Google Gemini API:** Currently, Google offers a generous Free Tier for the 1.5 Flash model (15 RPM). Perfect for free labs.
- **Azure Document Intelligence:** Charges around `$10` for every 1,000 invoice pages successfully analyzed (Pay-as-you-go). The Free Tier (F0) allows 500 free pages/month.
