---
title: "Week 8 Worklog"
date: 2026-07-06
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives

* Analyze Hybrid Access solutions (On-premises to AWS) using Interface VPC Endpoints powered by AWS PrivateLink technology.
* Practice provisioning and configuring an Interface VPC Endpoint for Amazon S3 following Workshop Chapter 5.4 ("Accessing S3 from On-premises").
* Master granular access governance using VPC Endpoint Policies based on Workshop Chapter 5.5 ("VPC Endpoint Policies").
* Write custom JSON Endpoint Policies restricting S3 actions exclusively to corporate buckets while blocking external access.
* Execute resource teardown procedures following Chapter 5.6 ("Cleanup") to optimize cloud spending and finalize Chapter 5 report.

### Tasks Completed During the Week

| Day | Tasks | Start Date | Completion Date | Learning Resources |
| --- | --- | --- | --- | --- |
| Monday | - Deep-dive into AWS PrivateLink technology and Interface VPC Endpoints.<br>- Understand internal architecture: Elastic Network Interfaces (ENI) assigned with private IPs inside subnets using Private DNS.<br>- Compare hybrid routing patterns from On-Premises environments (via Direct Connect/VPN) for Gateway Endpoints vs Interface Endpoints. | 06/07/2026 | 06/07/2026 | [AWS PrivateLink Concepts](https://docs.aws.amazon.com/vpc/latest/privatelink/concepts.html)<br>[Interface Endpoints for S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/privatelink-interface-endpoints.html) |
| Tuesday | - Execute Chapter 5.4 lab: Create an Interface VPC Endpoint for Amazon S3 via AWS Console.<br>- Configure ENI Security Group restricting access to HTTPS port 443 from simulated On-Premises CIDR ranges.<br>- Practice DNS Endpoint setup and inspect private DNS resolution via `nslookup` / `dig` to confirm ENI private IP addresses. | 07/07/2026 | 07/07/2026 | [Workshop S3 On-Premises](5.4-S3-onprem/)<br>[Private DNS for Endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-interface-endpoints.html) |
| Wednesday | - Study Chapter 5.5: VPC Endpoint Policies - an additional defense-in-depth layer for VPC Endpoints.<br>- Analyze authorization evaluation order across IAM Policies, S3 Bucket Policies, and VPC Endpoint Policies.<br>- Draft a custom JSON Endpoint Policy restricting Principal access solely to organization account resources. | 08/07/2026 | 08/07/2026 | [Workshop Endpoint Policies](5.5-Policy/)<br>[VPC Endpoint Policies](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html) |
| Thursday | - **Endpoint Policy Verification Testing:**<br>- Test `aws s3 ls s3://allowed-company-bucket` -> Success.<br>- Test `aws s3 ls s3://external-personal-bucket` -> Access Denied by Endpoint Policy.<br>- Validate Data Exfiltration Prevention capabilities enforced through VPC Endpoint Policies. | 09/07/2026 | 09/07/2026 | [Workshop Endpoint Policies](5.5-Policy/)<br>[Preventing Data Exfiltration](https://aws.amazon.com/blogs/security/how-to-use-vpc-endpoint-policies-to-prevent-data-exfiltration/) |
| Friday | - **Resource Teardown & Report Finalization:**<br>- Execute teardown procedures per Chapter 5.6 ("Cleanup"): Delete VPC Endpoints, ENIs, EC2 instances, and test S3 buckets.<br>- Verify AWS Billing dashboard to ensure zero lingering hourly charges for Interface Endpoints.<br>- Finalize code snippets, screenshot evidence, and documentation for Chapter 5 of the report. | 10/07/2026 | 10/07/2026 | [Workshop Cleanup](5.6-Cleanup/)<br>[AWS Cost Allocation](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) |

### Week 8 Achievements

* Mastered AWS PrivateLink and Interface VPC Endpoint mechanics leveraging ENIs and Private DNS name resolution.
* Recognized Interface VPC Endpoints as the primary architecture for enabling secure On-Premises access to AWS S3 via private VPN/Direct Connect link.
* Provisioned an Interface VPC Endpoint for Amazon S3, configured ENI Security Groups, and validated DNS queries.
* Authored custom JSON VPC Endpoint Policies to enforce perimeter security controls on S3 resource access.
* Successfully validated Data Exfiltration Prevention by verifying that unauthorized requests to external S3 buckets were blocked at the endpoint boundary.
* Executed complete lab teardown according to Chapter 5.6, ensuring zero unexpected cloud costs or orphaned resources.
* Fully completed Chapter 5 Workshop documentation ("Securing Hybrid Access to S3 using VPC Endpoint") on the Hugo report template.
