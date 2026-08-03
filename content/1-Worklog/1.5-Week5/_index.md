---
title: "Week 5 Worklog"
date: 2026-06-15
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives

* Understand centralized monitoring and logging concepts on AWS using Amazon CloudWatch (Metrics, Logs, Alarms, Dashboards).
* Configure CloudWatch Alarms integrated with Amazon Simple Notification Service (SNS) for automated email/SMS incident alerts.
* Study AWS CloudTrail for system auditing, tracking API call history, and enforcing security governance.
* Master AWS Cost Management tools including AWS Cost Explorer and AWS Budgets to set up threshold alert notifications.
* Practice installing CloudWatch Agent on EC2 instances, shipping application logs to CloudWatch Logs, and building unified dashboards.

### Tasks Completed During the Week

| Day | Tasks | Start Date | Completion Date | Learning Resources |
| --- | --- | --- | --- | --- |
| Monday | - Explore Amazon CloudWatch fundamentals: Metrics, Log Groups, Log Streams, Alarms, and Dashboards.<br>- Differentiate Basic Monitoring (5-minute frequency) vs Detailed Monitoring (1-minute frequency) on EC2.<br>- Analyze critical system metrics: CPUUtilization, StatusCheckFailed, and NetworkIn/NetworkOut. | 15/06/2026 | 15/06/2026 | [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)<br>[CloudWatch Concepts](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html) |
| Tuesday | - Study Amazon SNS (Simple Notification Service) Pub/Sub messaging architecture.<br>- Create SNS Topic `CloudWatch-Alerts-Topic` and subscribe an Email endpoint.<br>- Practice creating a CloudWatch Alarm triggering when EC2 CPU exceeds 80% over 5 minutes and firing email alerts via SNS. | 16/06/2026 | 16/06/2026 | [Amazon SNS User Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)<br>[CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) |
| Wednesday | - Study AWS CloudTrail for capturing API event history and audit trails.<br>- Analyze the structure of CloudTrail Events (User Identity, Event Time, Event Source, Source IP, Request Parameters).<br>- Configure a multi-region CloudTrail Trail delivering all API history events to an Amazon S3 Bucket for secure archiving. | 17/06/2026 | 17/06/2026 | [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)<br>[CloudTrail Events](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html) |
| Thursday | - Explore AWS Cost Management tools and billing governance.<br>- Utilize AWS Cost Explorer to analyze infrastructure expenditure by service and region.<br>- Configure AWS Budgets to establish monthly budget limits and set up alert notifications when spending exceeds 80% of budget forecasts. | 18/06/2026 | 18/06/2026 | [AWS Cost Explorer](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-whatis.html)<br>[AWS Budgets Guide](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) |
| Friday | - **Practice & System Monitoring:**<br>- Practice installing the CloudWatch Unified Agent on Linux EC2 instances.<br>- Configure Agent to collect custom memory/disk metrics and ship system logs (`/var/log/syslog`) to CloudWatch Logs.<br>- Design a custom CloudWatch Dashboard displaying real-time metrics for CPU, Memory, Disk, and RDS Connections.<br>- Archive screenshot evidence and lab results. | 19/06/2026 | 19/06/2026 | [CloudWatch Agent Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html)<br>[CloudWatch Dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) |

### Week 5 Achievements

* Mastered the structure and operational principles of centralized monitoring via Amazon CloudWatch.
* Successfully created an Amazon SNS Topic and configured Email Subscriptions for instantaneous alert delivery.
* Configured a production-ready CloudWatch Alarm monitoring CPU Utilization with automated email alerts triggered upon high resource consumption.
* Understood AWS CloudTrail for security compliance, user activity tracking, and immutable audit logging in Amazon S3.
* Mastered AWS Cost Explorer and established AWS Budget threshold alerts to mitigate accidental cloud cost overruns.
* Successfully deployed CloudWatch Agent on EC2, capturing custom memory/disk metrics and aggregating application logs into CloudWatch Logs.
* Built a comprehensive CloudWatch Dashboard providing single-pane-of-glass visibility into overall system health (EC2, RDS, Network).
