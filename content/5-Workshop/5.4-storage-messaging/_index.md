---
title: "Database, Storage & Secrets"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---


In this module, we will provision the data persistence layer. Because we are building a production-ready application, we replace standard databases and basic config files with AWS SQL Server and Parameter Store.

## 1. Amazon RDS for SQL Server

Snaptics requires a robust, highly available database. SQL Server automatically replicates your data across multiple Availability Zones, ensuring zero data loss if a data center goes offline.

### A. Create DB Subnet Group
- Open **Amazon RDS ➔ Subnet groups ➔ Create DB subnet group**.
- **Name:** `snaptics-db-subnet-group`.
- **VPC:** `snaptics-vpc`.
- **Subnets:** Select your 2 Availability Zones and check ONLY the **2 Private Subnets** (`10.0.3.0/24` and `10.0.4.0/24`).

### B. Create SQL Server Cluster
- Open **RDS ➔ Databases ➔ Create database**.
- **Engine options:** Select **SQL Server**.
- **Choose a database creation method:** **Full configuration**.
- **Templates:** **Dev/Test**.
- **Settings:**
  - DB cluster identifier: `snaptics-sql-server`
  - Master username: `admin`
  - Master password: Generate a strong password (e.g., `SnapticsAurora2024!`).
- **Instance configuration:**
  - Select **Burstable classes (includes t classes)**.
  - Instance type: `db.t3.micro`.
- **Storage:**
  - Storage type: **General purpose SSD (gp3)**.
  - Allocated Storage: `200`.
  - Provisioned IOPS: `3000`.
  - Storage throughput: `125`.
- **Connectivity:**
  - Compute resource: select **Don't connect to an EC2 compute resource**.
  - VPC: `snaptics-vpc`.
  - **Public access: No** (Crucial for security).
  - VPC security group: select **Choose existing**, then choose `default`.
  - Availability Zone: **No preference**.
- Keep all other settings as default, then click **Create database**. Wait ~15 minutes and copy the **Writer Endpoint**.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.4-storage-messaging/aurora_and_rds_create_db.png" >
  </div>
  
## 2. Secure Storage (Amazon S3)

Invoice images must be stored efficiently. Since we configured a **VPC Gateway Endpoint** in the previous step, our containers can save files to S3 internally without internet bandwidth costs.

- Open **Amazon S3 ➔ Create bucket**.
- **Bucket name:** `s3-bucket-snaptics` (Must be globally unique).
- **Region:** `ap-southeast-1`.
- **Block Public Access:** **ON** (Keep invoices 100% private).
- **CORS Configuration (Permissions tab):** Allows frontend applications hosted on AWS Amplify to fetch pre-signed URLs directly.
```json
[
    {
        "AllowedMethods": [ "GET", "PUT", "POST" ],
        "AllowedOrigins": [ "*" ],
        "AllowedHeaders": [ "*" ]
    }
]
```
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.4-storage-messaging/amazon_s3_create.png" >
  </div>

## 3. Parameter Management (AWS Systems Manager Parameter Store)

NEVER hardcode your SQL Server DB password or AI API Keys in your GitHub repo! We will use **AWS Systems Manager Parameter Store** to store these securely in a hierarchical structure.

- Open **AWS Systems Manager ➔ Parameter Store ➔ Create parameter**.
- Create the following parameters one by one. For passwords/keys, choose Type **SecureString** to encrypt them (AWS automatically uses KMS). For normal strings, choose Type **String**:

  - `/Snaptics/Production/AWS/AccessKey` (Type: **SecureString**)
  - `/Snaptics/Production/AWS/SecretKey` (Type: **SecureString**)
  - `/Snaptics/Production/AiSettings/AzureDocIntelKey` (Type: **SecureString**)
  - `/Snaptics/Production/AiSettings/GeminiApiKey` (Type: **SecureString**)
  - `/Snaptics/Production/AwsSns/TopicArn` (Type: **String**)
  - `/Snaptics/Production/ConnectionStrings/DefaultConnection` (Type: **SecureString**)
  - `/Snaptics/Production/EmailSettings/Email` (Type: **SecureString**)
  - `/Snaptics/Production/EmailSettings/Password` (Type: **SecureString**)
  - `/Snaptics/Production/TokenKey` (Type: **SecureString**)

In your `.NET` code, thanks to the AWS SDK, the application will automatically fetch these parameters using the `/Snaptics/Production/*` path to override `appsettings.json`, keeping your source code completely clean and secure.