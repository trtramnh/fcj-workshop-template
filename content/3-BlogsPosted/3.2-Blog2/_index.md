---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# BUILDING A SCALABLE E-COMMERCE WEBSITE ON AWS

Modern e-commerce websites often experience significant traffic fluctuations, especially during promotional campaigns, flash sales, and peak shopping seasons. If every request is handled by a single application server while directly accessing the database, the system can quickly become overloaded, resulting in slower response times or service interruptions.

AWS provides a wide range of managed services that help organizations build scalable, secure, and highly available web applications. By combining networking, security, compute, caching, databases, and monitoring services, developers can design cloud-native architectures capable of handling dynamic workloads while maintaining a consistent user experience.

### Architecture Overview

The overall request flow is illustrated below:

**User → Amazon Route 53 → Amazon CloudFront → AWS WAF → Application Load Balancer → Amazon ECS (AWS Fargate) → Amazon ElastiCache / Amazon Aurora Serverless v2**

Each AWS service plays a specific role within the architecture:

- **Amazon Route 53**
  - Resolves domain names and routes user requests to the appropriate AWS resources.

- **Amazon CloudFront**
  - Delivers content from edge locations closer to users, reducing latency and improving website performance.

- **AWS WAF**
  - Protects the application against common web attacks such as SQL injection and cross-site scripting (XSS).

- **Application Load Balancer**
  - Distributes incoming requests across multiple backend containers to improve scalability and availability.

- **Amazon ECS with AWS Fargate**
  - Runs containerized backend services without requiring developers to manage servers.

- **Amazon Cognito**
  - Handles user registration, authentication, and authorization securely.

- **Amazon ElastiCache**
  - Stores frequently accessed data in memory to reduce database workload and improve response times.

- **Amazon Aurora Serverless v2**
  - Stores core application data while automatically scaling database capacity based on workload.

### Monitoring and Alerting

Monitoring plays an important role in maintaining application reliability.

Amazon CloudWatch continuously collects metrics and logs from Amazon ECS and Amazon Aurora. When abnormal conditions are detected, such as high CPU utilization, application errors, or database performance degradation, **CloudWatch Alarm** automatically triggers **Amazon SNS** to send notifications via email or SMS.

**Monitoring Flow**

**Amazon CloudWatch → CloudWatch Alarm → Amazon SNS → Email / SMS**

### Benefits of the Architecture

This architecture offers several advantages:

- Improved application performance through CloudFront and ElastiCache.
- Enhanced security using AWS WAF and Amazon Cognito.
- Automatic scalability with Amazon ECS, AWS Fargate, and Aurora Serverless v2.
- High availability through Application Load Balancer.
- Continuous monitoring and proactive alerting using CloudWatch and Amazon SNS.

### What I learned

This reference architecture demonstrates how multiple AWS managed services can be integrated to build a scalable and cloud-native e-commerce platform.

By exploring this architecture, I gained a better understanding of the responsibilities of individual AWS services and how networking, security, container orchestration, caching, databases, and monitoring work together to support production-ready applications. It also reinforced the importance of designing systems that are scalable, secure, and resilient from the beginning.

### Images

<div style="text-align: center;">
    <img src="/fcj-workshop-template/images/3-BlogsPosted/3.2-Blog2/blog2.jpg"
         alt="Scalable E-commerce Architecture"
         style="width: 900px; height: auto; border-radius: 8px;">
    <p>Scalable E-commerce website architecture on AWS.</p>
</div>

### Reference Material

This blog is based on the following AWS official guidance:

- **Guidance for Web Store on AWS**
  https://docs.aws.amazon.com/solutions/web-store-on-aws/

- **Guidance for Building a Containerized and Scalable Web Application on AWS**
  https://docs.aws.amazon.com/solutions/building-a-containerized-and-scalable-web-application-on-aws/