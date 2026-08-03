---
title: "Blog 1"
date: 2026-07-27
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# SCALABLE E-COMMERCE WEBSITE ARCHITECTURE ON AWS

Hello everyone,

E-commerce websites often experience huge fluctuations in traffic, especially during promotional events or peak shopping seasons. If all requests are handled by a single server and query the database directly, the system can easily become slow, overloaded, or interrupted.

## General Architecture Flow

```text
User → Route 53 → CloudFront → AWS WAF → Application Load Balancer → ECS Fargate → ElastiCache/Aurora
```

![Scalable E-commerce Web Application Architecture on AWS](/images/3.1-Blog1/blog1.jpg)

## How the System Works

1. **Amazon Route 53**

   Routes user requests to the system.

2. **Amazon CloudFront**

   Delivers content from edge locations close to users, reducing latency and offloading the backend infrastructure.

3. **AWS WAF**

   Inspects and blocks anomalous requests before they reach the application.

4. **Application Load Balancer**

   Distributes valid requests to Backend containers running on Amazon ECS.

5. **Amazon ECS with AWS Fargate**

   Runs Backend containers without directly managing servers. The system can scale the number of containers up or down based on demand.

6. **Amazon Cognito**

   Supports user registration, sign-in, and authentication. Cognito is an authentication service and does not reside directly on the public request processing path.

7. **Amazon ElastiCache**

   Caches frequently accessed data to speed up response times and reduce direct queries to the database.

8. **Amazon Aurora Serverless v2**

   Stores core website data such as user profiles, products, inventory, and orders. Aurora Serverless v2 can automatically adjust resources according to workload demand.

## System Monitoring and Alerting

Amazon CloudWatch monitors the activity of ECS and Aurora. When high CPU utilization, frequent application errors, or abnormal database resource usage are detected, CloudWatch Alarms trigger Amazon SNS to send notifications via email or SMS.

```text
CloudWatch → CloudWatch Alarm → Amazon SNS → Email/SMS
```

## Architectural Benefits

By combining these services, the website can:

* Increase access speed for users.
* Enhance security.
* Reduce database load.
* Scale flexibly when traffic spikes.
* Automatically monitor and detect issues early.
* Minimize the risk of downtime during promotions or peak shopping seasons.

## Reference Material

* [Guidance for Web Store on AWS](https://docs.aws.amazon.com/solutions/web-store-on-aws/)
* [Guidance for Building a Containerized and Scalable Web Application on AWS](https://docs.aws.amazon.com/solutions/building-a-containerized-and-scalable-web-application-on-aws/)

#AWS #AWSArchitecture #CloudComputing #Ecommerce #ECS #Fargate #CloudFront #Aurora #CloudWatch