---
title: "Week 11 Worklog"
date: 2026-07-27
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives

* Draft and finalize Blog 1: "Scalable E-Commerce Architecture on AWS" (Analyzing ECS Fargate, ALB, CloudFront, Aurora Serverless v2, ElastiCache).
* Draft and finalize Blog 2: "AWS Journey & Cloud-Native Mindset from Real Internship" (Sharing hands-on experiences with Amazon S3, AWSSDK.S3, Decoupling, IAM Least Privilege, Pre-signed URLs, CORS).
* Draft and finalize Blog 3: "How AWS Upgraded Amazon Cognito with Zero Downtime" (Analyzing Zero Downtime Migration architecture, Dual-write, Anti-entropy validation).
* Publish full bilingual versions (Vietnamese and English) for all 3 blogs into the Hugo workspace (`content/3-BlogsPosted/`) with high-resolution diagrams.
* Review and update internal cross-links connecting Worklogs, Proposal, Workshop, and Blogs Posted within the Hugo report interface.

### Tasks Completed During the Week

| Day | Tasks | Start Date | Completion Date | Learning Resources |
| --- | --- | --- | --- | --- |
| Monday | - Write content for Blog 1 covering Scalable E-Commerce Web Architecture on AWS.<br>- Analyze end-to-end request flow: Route 53 -> CloudFront -> AWS WAF -> ALB -> ECS Fargate -> ElastiCache / Aurora Serverless v2.<br>- Detail system monitoring and automated notification mechanisms via CloudWatch Alarms and Amazon SNS. | 27/07/2026 | 27/07/2026 | [Blog 1 Overview](3-BlogsPosted/3.1-Blog1/)<br>[AWS Scalable Web Guidance](https://docs.aws.amazon.com/solutions/building-a-containerized-and-scalable-web-application-on-aws/) |
| Tuesday | - Write content for Blog 2 summarizing practical experiences interacting with Amazon S3 during backend development.<br>- Detail compute and storage Decoupling principles using AWSSDK.S3 in C#/.NET.<br>- Share real-world lessons on IAM Least Privilege, securing Secret Keys via Environment Variables, generating Pre-signed URLs, and resolving CORS issues. | 28/07/2026 | 28/07/2026 | [Blog 2 Overview](3-BlogsPosted/3.2-Blog2/)<br>[Amazon S3 Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) |
| Wednesday | - Write content for Blog 3 analyzing AWS's modern infrastructure upgrade of Amazon Cognito.<br>- Highlight new capabilities: High-throughput performance, Customer-managed keys (KMS), and Multi-Region replication.<br>- Analyze zero-downtime migration patterns: Shadow mode validation, Dual-write architecture, and Anti-entropy validation. | 29/07/2026 | 29/07/2026 | [Blog 3 Overview](3-BlogsPosted/3.3-Blog3/)<br>[AWS Cognito Infrastructure Upgrade Blog](https://aws.amazon.com/blogs/security/amazon-cognito-unlocks-advanced-capabilities-with-next-generation-infrastructure/) |
| Thursday | - Format Markdown layout and embed architecture diagrams across all 3 blog posts within Hugo.<br>- Verify image file paths inside `static/images/` to ensure crisp rendering across both Vietnamese and English pages.<br>- Audit technical hashtags and official AWS reference documentation links for each blog post. | 30/07/2026 | 30/07/2026 | [Blogs Posted Directory](3-BlogsPosted/)<br>[Hugo Content Management](https://gohugo.io/content-management/formats/) |
| Friday | - **Audit & System Synchronization:**<br>- Validate date metadata consistency (`date: 2026-07-27`) and weight ordering across all 3 blog posts.<br>- Verify internal cross-linking from Week 10–11 Worklog entries to corresponding blog posts.<br>- Commit complete article source code, ready for final internship evaluation. | 31/07/2026 | 31/07/2026 | [Blogs Posted Directory](3-BlogsPosted/)<br>[Hugo Shortcodes](https://gohugo.io/content-management/shortcodes/) |

### Week 11 Achievements

* Published Blog 1 sharing scalable E-Commerce Web architecture design, capable of handling flash sales with automated CloudWatch/SNS monitoring.
* Published Blog 2 distilling hands-on experiences with Amazon S3, decoupled system design, AWSSDK.S3 programming, IAM security, and Pre-signed URLs.
* Published Blog 3 analyzing AWS's zero-downtime infrastructure migration of Amazon Cognito utilizing Shadow Mode and Dual-write patterns.
* Synchronized bilingual documentation (`_index.vi.md` and `_index.md`) for all 3 blog posts in `content/3-BlogsPosted/`.
* Embedded high-resolution architectural diagrams in dedicated `images/` directories for enhanced visual clarity.
* Optimized internal navigation links and standardized Markdown formatting across the Hugo internship report.
