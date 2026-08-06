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

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iamuser_create_1.jpg" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_permission.jpg" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_create_2.jpg" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_create_complete.jpg" >
  </div>

### B. Generate Access Keys
1. Click on the newly created `github-actions-snaptics` user.
2. Go to the **Security credentials** tab.
3. Scroll down to **Access keys** and click **Create access key**.
4. Choose **Command Line Interface (CLI)**, check the confirmation box, and click Next.
5. Copy the **Access key ID** and **Secret access key**. **Save them securely!** You will need to paste these into GitHub Repository Secrets later.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_security_credentials.jpg" >
  </div>

<div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_security_credentials_2.jpg" >
  </div>

<div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_security_credentials_3.jpg" >
  </div>

## 3. AWS IAM Roles for ECS Containers

When the .NET code runs inside the ECS Fargate container, it needs permissions to talk to S3, SQS, SNS, and Parameter Store. We must create **2 specific IAM Roles**.

### A. Create ECS Task Execution Role (`ecsTaskExecutionRole`)

1. Open **IAM ➔ Roles ➔ Create role**.
2. Click **Choose a service or use case**.
3. Search for and select **Elastic Container Service**.
4. In the **Use case** section, select exactly **Elastic Container Service Task**. Do **NOT** select "EC2 Role for Elastic Container Service".
5. Click **Next**.

**In the Add permissions step:**

6. Search for `AmazonECSTaskExecutionRolePolicy` and tick that policy.
7. If you are passing the DB password from Parameter Store into the **Secrets** section of your ECS Task Definition, also search for and tick `AmazonSSMReadOnlyAccess`.
8. Click **Next**.

**Name, review, and create:**

9. Set the role name to `ecsTaskExecutionRole`.
10. Click **Create role**.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3a_ecs_role_create.jpg" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3a_ecs_role_create_1.jpg" >
  </div>

> [!NOTE]
> AWS confirms that the task execution role is used by ECS to **pull the image from ECR**, **write logs to CloudWatch**, and **retrieve Parameter Store/Secrets Manager** when the task definition references a secret.
>
> `AmazonSSMReadOnlyAccess` is quite broad. You can use it to get your deployment working first, then later replace it with an inline policy that only reads the `/Snaptics/Production/*` path.

### B. Create Snaptics ECS Task Role (`snaptics-ecs-task-role`)

1. Go back to **IAM ➔ Roles ➔ Create role**.
2. **Trusted entity type:** AWS service.
3. **Service or use case:** Elastic Container Service.
4. **Use case:** Elastic Container Service Task.
5. Click **Next**.
6. At **Add permissions**, you don't need to select any policy yet; click **Next**.
7. Set the role name to `snaptics-ecs-task-role`.
8. Click **Create role**.

**After the role is created:**

9. Open the `snaptics-ecs-task-role` role.
10. Select the **Permissions** tab.
11. Click **Add permissions ➔ Create inline policy**.
12. Switch to the **JSON** tab.
13. Delete the old content and paste the policy below. Replace `<ACCOUNT_ID>` with your AWS Account ID.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowParameterStore",
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameter",
                "ssm:GetParameters",
                "ssm:GetParametersByPath"
            ],
            "Resource": "arn:aws:ssm:ap-southeast-1:<ACCOUNT_ID>:parameter/Snaptics/Production/*"
        },
        {
            "Sid": "AllowS3BucketListing",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::s3-bucket-snaptics"
        },
        {
            "Sid": "AllowS3Objects",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::s3-bucket-snaptics/*"
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

14. Click **Next**.
15. Set the policy name to `SnapticsRuntimePolicy`.
16. Click **Create policy**.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_1.jpg" >
  </div>

<div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_2.jpg" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_3.jpg" >
  </div>  

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_4.jpg" >
  </div>  

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_5.jpg" >
  </div>  

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_6.jpg" >
  </div>  

> [!NOTE]
> The task role is the permission your **C# code inside the container** uses to call S3, SQS, SNS, or Parameter Store. It is separate from the permissions ECS itself uses to start the container.

### C. Assign the Roles in the ECS Task Definition

When creating your ECS Task Definition, fill in these two fields correctly:

| Field | Value |
|-------|-------|
| **Task role** | `snaptics-ecs-task-role` |
| **Task execution role** | `ecsTaskExecutionRole` |

The difference is:

- `ecsTaskExecutionRole` ➔ ECS **pulls the image**, **creates logs**, and **injects secrets** when the container starts.
- `snaptics-ecs-task-role` ➔ The **running C# code** calls S3, SQS, SNS, SSM.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3c_ecs_assign_role_1.jpg" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3c_ecs_assign_role_2.jpg" >
  </div> 

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3c_ecs_assign_role_3.jpg" >
  </div>   

> [!TIP]
> If Parameter Store uses a **SecureString with a customer-managed KMS key**, the role that actually reads the parameter also needs `kms:Decrypt` permission for that KMS key. If the password is configured in the **Secrets** section of the task definition, this permission belongs to the **execution role**; if C# calls SSM itself using the AWS SDK, it belongs to the **task role**.