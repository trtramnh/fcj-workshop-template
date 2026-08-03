---
title: "Week 10 Worklog"
date: 2026-07-20
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives

* Configure AWS IoT Core Topic Rules to automatically store edge telemetry into Amazon S3 Raw Data Lake Buckets.
* Build AWS Lambda functions (Python/Node.js) handling backend logic and set up Amazon API Gateway RESTful endpoints.
* Configure AWS Glue Crawlers to catalog S3 schemas automatically and create Glue ETL Jobs for data transformation into analytical Parquet format.
* Deploy a Next.js Web Dashboard onto AWS Amplify, integrating Amazon Cognito User Pools for researcher authentication.
* Research advanced scalable cloud architecture (Amazon ECS Fargate, ALB, Aurora Serverless v2, ElastiCache) in preparation for technical blogging.

### Tasks Completed During the Week

| Day | Tasks | Start Date | Completion Date | Learning Resources |
| --- | --- | --- | --- | --- |
| Monday | - Configure AWS IoT Core Rule subscribing to topic `lab/weather/telemetry`.<br>- Configure S3 action writing JSON payloads directly into Amazon S3 Raw Bucket partitioned by `year=YYYY/month=MM/day=DD/`.<br>- Test publishing simulated MQTT messages and verify automated S3 object creation. | 20/07/2026 | 20/07/2026 | [AWS IoT S3 Action](https://docs.aws.amazon.com/iot/latest/developerguide/s3-rule-action.html)<br>[AWS IoT Rule Tutorial](https://docs.aws.amazon.com/iot/latest/developerguide/iot-write-to-s3.html) |
| Tuesday | - Program AWS Lambda Function to query the latest weather telemetry data.<br>- Create Amazon API Gateway REST API integrating with Lambda via GET `/api/weather/latest`.<br>- Configure CORS headers and verify endpoint response using Postman. | 21/07/2026 | 21/07/2026 | [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)<br>[API Gateway Integration](https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started-with-lambda-integrations.html) |
| Wednesday | - Create AWS Glue Crawler to inspect S3 Raw Bucket periodically and extract schema metadata into AWS Glue Data Catalog.<br>- Write PySpark Glue ETL Job filtering telemetry noise and exporting to S3 Processed Bucket in compressed Parquet format for fast queries.<br>- Trigger Glue Job and verify transformed datasets on S3. | 22/07/2026 | 22/07/2026 | [AWS Glue Crawlers](https://docs.aws.amazon.com/glue/latest/dg/add-crawler.html)<br>[AWS Glue ETL Jobs](https://docs.aws.amazon.com/glue/latest/dg/author-job.html) |
| Thursday | - Create Amazon Cognito User Pool `WeatherLabUserPool` and configure App Client credentials.<br>- Deploy Next.js Web Dashboard to AWS Amplify via Git repository integration.<br>- Integrate Amplify Auth SDK into Next.js dashboard UI to secure weather monitoring views. | 23/07/2026 | 23/07/2026 | [AWS Amplify Hosting](https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html)<br>[Amplify Authentication](https://docs.amplify.aws/lib/auth/getting-started/q/platform/js/) |
| Friday | - **Research Scalable Web Application Architecture:**<br>- Analyze high-traffic ecommerce scalability challenges.<br>- Study end-to-end traffic flow: Route 53 -> CloudFront -> AWS WAF -> ALB -> Amazon ECS Fargate -> ElastiCache -> Aurora Serverless v2.<br>- Gather AWS Solutions Guidance references to draft Technical Blog 1. | 24/07/2026 | 24/07/2026 | [AWS Web Store Guidance](https://docs.aws.amazon.com/solutions/web-store-on-aws/)<br>[Amazon ECS Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html) |

### Week 10 Achievements

* Deployed AWS IoT Core Rules to capture MQTT telemetry automatically and partition raw JSON objects into Amazon S3.
* Developed AWS Lambda function logic and API Gateway endpoints to serve real-time weather metrics to web clients.
* Configured AWS Glue Crawlers and Glue ETL Jobs to transform raw IoT data into optimized analytical Parquet files on S3.
* Deployed Next.js frontend application onto AWS Amplify and completed integration with Amazon Cognito User Pools for access security.
* Deepened technical knowledge regarding enterprise scalable web architectures (Amazon ECS Fargate, ALB, Aurora Serverless v2, ElastiCache).
* Prepared architectural diagrams and theoretical content for publishing Technical Blog 1 on Scalable E-Commerce Architecture.
