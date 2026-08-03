---
title: "Week 3 Worklog"
date: 2026-06-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives

* Master Amazon S3 Object Storage concepts, bucket structures, object keys, metadata, and versioning.
* Differentiate S3 Storage Classes (Standard, Intelligent-Tiering, Standard-IA, Glacier) for data lifecycle cost optimization.
* Master S3 security best practices including Block Public Access, Bucket Policies, ACLs, and Server-Side Encryption (SSE-S3, SSE-KMS).
* Grasp AWS IAM core components (Users, Groups, Roles, Policies) and enforce the Principle of Least Privilege.
* Integrate AWSSDK.S3 into a C#/.NET Backend application, implement Pre-signed URLs, configure CORS, and set up S3 Lifecycle Policies.

### Tasks Completed During the Week

| Day | Tasks | Start Date | Completion Date | Learning Resources |
| --- | --- | --- | --- | --- |
| Monday | - Explore Amazon S3 overview and object storage fundamentals.<br>- Study core components: Buckets, Objects, Keys, Metadata, and Object Versioning.<br>- Analyze S3 Storage Classes (Standard, Intelligent-Tiering, Standard-IA, Glacier) and cost optimization strategies. | 01/06/2026 | 01/06/2026 | [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)<br>[S3 Storage Classes](https://aws.amazon.com/s3/storage-classes/) |
| Tuesday | - Study Amazon S3 security mechanisms.<br>- Practice enabling Block Public Access to prevent accidental internet data exposure.<br>- Write granular JSON S3 Bucket Policies to control resource access.<br>- Configure data encryption at rest using Server-Side Encryption (SSE-S3 and SSE-KMS). | 02/06/2026 | 02/06/2026 | [Amazon S3 Security](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html)<br>[S3 Bucket Policies](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-policies.html) |
| Wednesday | - Study AWS Identity and Access Management (IAM) fundamentals.<br>- Differentiate IAM Users, Groups, Roles, and Policies.<br>- Learn the Principle of Least Privilege in cloud security governance.<br>- Practice creating a dedicated application IAM User with JSON policy restricted strictly to `s3:GetObject` and `s3:PutObject`. | 03/06/2026 | 03/06/2026 | [AWS IAM Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)<br>[IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) |
| Thursday | - Learn how to integrate AWSSDK.S3 into C#/.NET Backend projects.<br>- Build S3Service wrapper handling `PutObjectAsync`, `GetObjectAsync`, and `DeleteObjectAsync` methods.<br>- Implement Pre-signed URLs technique for secure, time-bounded (15–30 min) file sharing.<br>- Securely manage AWS Access Key and Secret Key using Environment Variables. | 04/06/2026 | 04/06/2026 | [AWS SDK for .NET S3 Guide](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/s3-index.html)<br>[Pre-signed URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ShareObjectPreSignedURL.html) |
| Friday | - **Practice & Troubleshooting:**<br>- Diagnose and fix CORS errors when the frontend browser invokes Pre-signed URLs for file uploads.<br>- Configure S3 Bucket CORS rules properly (AllowedOrigins, AllowedMethods).<br>- Configure S3 Lifecycle Rules to automatically purge orphan files in the `temp/` folder after 7 days.<br>- Capture screenshot evidence and record lab results. | 05/06/2026 | 05/06/2026 | [S3 CORS Configuration](https://docs.aws.amazon.com/AmazonS3/latest/userguide/cors.html)<br>[S3 Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html) |

### Week 3 Achievements

* Acquired in-depth understanding of Amazon S3 Object Storage, buckets, objects, metadata, and versioning mechanics.
* Evaluated S3 Storage Classes and designed storage strategies for optimal performance and cost efficiency.
* Successfully implemented S3 security controls by turning on Block Public Access, writing custom Bucket Policies, and enabling SSE-S3/SSE-KMS encryption.
* Mastered core AWS IAM principles (Users, Groups, Roles, Policies) while strictly enforcing Least Privilege.
* Provisioned dedicated IAM credentials for application usage with scoped JSON policy permissions.
* Integrated AWSSDK.S3 package into C#/.NET backend codebase, supporting seamless upload, download, and deletion lifecycle.
* Implemented secure Pre-signed URLs to share private S3 objects securely without exposing public bucket access.
* Resolved browser CORS policy challenges for S3 file uploads and created S3 Lifecycle Rules for automated storage cleanup.
