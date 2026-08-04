---
title: "IAM Roles"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2.2. </b> "
---


For a .NET application running on an ECS container to communicate with other AWS services (like sending files to S3, reading environment variables from Parameter Store, or dropping messages into SQS), we need to grant valid access permissions (IAM Policies) instead of hardcoding AWS API Keys directly into the source code.

In the AWS Console, go to **IAM ➔ Roles ➔ Create role** and create 2 Roles with the detailed configuration below:

### 1. ECS Task Execution Role (`ecsTaskExecutionRole`)

This role grants permissions to the ECS platform itself. Its main task is to automatically pull Docker images from the Elastic Container Registry (ECR) and create Log Streams in Amazon CloudWatch.

- **Trusted Entity:** `Elastic Container Service Task`
- **Managed Policy (Pre-created by AWS):** 
  - `AmazonECSTaskExecutionRolePolicy`

The exact JSON code describing this Role's permissions:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "*"
        }
    ]
}
```

### 2. Snaptics ECS Task Role (`snaptics-ecs-task-role`)

This role grants permissions to **your own C# .NET code** executing inside the container.

- **Trusted Entity:** `Elastic Container Service Task`
- **Permissions:** Create a new Inline Policy and paste the following JSON snippet. It grants your app permissions to Read configs from SSM Parameter Store, Read/Write data to the S3 bucket, and process SQS and SNS queues.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowParameterStore",
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameters",
                "ssm:GetParametersByPath"
            ],
            "Resource": "arn:aws:ssm:ap-southeast-1:*:parameter/Snaptics/Production/*"
        },
        {
            "Sid": "AllowS3Storage",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::s3-bucket-snaptics",
                "arn:aws:s3:::s3-bucket-snaptics/*"
            ]
        },
        {
            "Sid": "AllowSQSSNS",
            "Effect": "Allow",
            "Action": [
                "sqs:SendMessage",
                "sqs:ReceiveMessage",
                "sqs:DeleteMessage",
                "sns:Publish"
            ],
            "Resource": "*"
        }
    ]
}
```

> [!TIP]
> By restricting the `Resource` to specific names (like `s3-bucket-snaptics` instead of `*`), our architecture strictly complies with the **Least Privilege** principle of large enterprise systems.
