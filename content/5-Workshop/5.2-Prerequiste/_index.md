---
title: "Prerequisites"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---


To ensure the deployment of Snaptics Enterprise goes smoothly, you must set up your Cloud environments properly. Since we are using an automated CI/CD approach instead of manual deployments, the requirements are heavily focused on Cloud permissions rather than installing local software.

## 1. Third-Party Accounts & APIs

Before touching AWS, make sure you have the following ready:

1. **GitHub Account:** We will use **GitHub Actions** to automate the build and deployment. You need a free GitHub account to fork the Snaptics source code repository.
2. **Google Gemini API Key:** Required for the AI financial consulting feature. Get it for free at [Google AI Studio](https://aistudio.google.com/app/apikey).
3. **Azure Document Intelligence Key:** Required for Invoice OCR. Log into the [Azure Portal](https://portal.azure.com/), create a **Document Intelligence** resource, and copy both the **Endpoint URL** and **Key 1**.

## 2. AWS IAM User for GitHub Actions

Since GitHub Actions will be deploying infrastructure on your behalf, it needs programmatic access to your AWS account.

### A. Create an IAM User
1. Log into the AWS Console, go to **IAM ➔ Users ➔ Create user**.
2. Name the user: `github-actions-snaptics`.
3. Do **NOT** check "Provide user access to the AWS Management Console" (this is a programmatic user only).
4. Click Next. Under Permissions, choose **Attach policies directly**.
5. Attach the `AdministratorAccess` policy. *(Note: In a real strict enterprise environment, you should only grant specific ECS/ECR/S3 permissions. For the sake of this workshop, Admin access simplifies the pipeline setup).*
6. Complete the creation.

### B. Generate Access Keys
1. Click on the newly created `github-actions-snaptics` user.
2. Go to the **Security credentials** tab.
3. Scroll down to **Access keys** and click **Create access key**.
4. Choose **Command Line Interface (CLI)**, check the confirmation box, and click Next.
5. Copy the **Access key ID** and **Secret access key**. **Save them securely!** You will need to paste these into GitHub Repository Secrets later.


  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iamuser_create_1.png" >
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_create_2.png" >
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_create_complete.png" >
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_permission.png" >
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_security_credentials.png" >
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_security_credentials_2.png" >
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_security_credentials_3.png" >

## 3. AWS IAM Roles for ECS Containers

When the .NET code runs inside the ECS Fargate container, it needs permissions to talk to S3, SQS, and Parameter Store. We must create 2 specific IAM Roles.

Go to **IAM ➔ Roles ➔ Create role**:

### A. ECS Task Execution Role (`ecsTaskExecutionRole`)
This role allows the underlying ECS platform to pull your Docker image from ECR and stream logs to CloudWatch.
- **Trusted Entity:** `Elastic Container Service Task`
- **Managed Policy:** Search for and attach `AmazonECSTaskExecutionRolePolicy`.
- Also attach `AmazonSSMReadOnlyAccess` so it can fetch the DB password during container startup.


  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3a_ecs_role_create.png" >
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3a_ecs_role_create_1.png" >

### B. Snaptics ECS Task Role (`snaptics-ecs-task-role`)
This role grants permissions to your **C# code** executing inside the container.
- **Trusted Entity:** `Elastic Container Service Task`
- **Inline Policy (JSON):** Create an inline policy and paste the following JSON to grant access to S3, SQS, SNS, and Parameter Store.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowParameterStore",
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameter",
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
> By strictly defining these roles, we follow the **Principle of Least Privilege**. If a hacker somehow gains access to the container, they still cannot delete your SQL Server database because this role has no RDS deletion permissions!
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_1.png" >
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_2.png" >
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_3.png" >
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_4.png" >
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_5.png" >
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_6.png" >

### C. Assign Role
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3c_ecs_assign_role_1.png" >
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3c_ecs_assign_role_2.png" >
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3c_ecs_assign_role_3.png" >