---
title: "Blog 2"
date: 2026-07-27
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# AWS TOOLKIT & CLOUD-NATIVE MINDSET FROM REAL-WORLD INTERNSHIP

Hello everyone,

During my internship and participation in practical project development at the company, although the main Cloud component I interacted with was Amazon S3, the implementation process helped me acquire essential foundational knowledge about the Amazon Web Services (AWS) ecosystem and Cloud-Native mindset.

![Architecture Diagram: Monolithic Storage to S3 Decoupled Conversion](/images/3.2-Blog2/architecture-diagram.jpg)

## Key Learnings & Practical Application

1. **Understanding and Applying System Decoupling**

   * **Key Learning:** Previously, I used to store static files (images, documents) locally on the application server (Local storage) or as binary data inside the database. Working with AWS helped me understand the "Decoupling" mindset — completely separating Compute from Storage.
   * **Practical Application:** I designed the .NET Backend to only handle business logic, offloading all files/documents to Amazon S3 (Object Storage) and storing only file URLs in the Database. This significantly reduced server load, eliminated disk capacity concerns, and kept database queries lightweight and fast.

2. **Mastering AWSSDK and Hands-on Operations with S3**

   * **Key Learning:** Transitioning from manual operations on the AWS Web Console to interacting with and managing resources entirely via code (using AWS SDK).
   * **Practical Application:** I successfully integrated the `AWSSDK.S3` NuGet package into the core system, writing custom Service classes in C# to manage the complete file lifecycle: streaming data uploads (`PutObjectRequest`), setting accurate Metadata/ContentType for different file formats, and securely retrieving and deleting files on the cloud.
   * **Cost Optimization Mindset:** Beyond file storage, I realized controlling orphaned files on S3 is equally crucial. In cases where users cancel uploads midway (e.g., closing tab, network drop), orphaned files remain on the Bucket without Database references. I implemented cleanup logic by invoking `DeleteObjectAsync` immediately upon transaction failures, or configuring S3 Lifecycle Policies to automatically delete temporary directory files (`temp/`) after a set period. This taught me a practical Cloud Cost Optimization lesson — pay only for what you use, but proactively clean up to avoid hidden storage costs.

3. **Access Control Management with AWS IAM (Identity and Access Management)**

   * **Key Learning:** Mastering the core principle of Cloud Security: Least Privilege Principle. I realized the severe security risks of using root AWS accounts for application connections.
   * **Practical Application:** I configured dedicated IAM User accounts for development environments. Instead of granting Admin access, I learned to write JSON IAM Policies to restrict permissions: the Backend application is only allowed to Read (`s3:GetObject`) and Write (`s3:PutObject`) to a single specific project Bucket, eliminating risks of data tampering or cross-bucket leaks.

4. **Secret Key & Environment Variable Security**

   * **Key Learning:** Clear awareness of Security Vulnerabilities when working with Cloud services, especially hardcoding credentials in source code.
   * **Practical Application:** I established a sensitive data management workflow for Access Keys and Secret Keys using `appsettings.json` (excluded from Git) and Environment Variables. This ensured absolute security when pushing source code to repositories like GitHub.

5. **Secure Data Sharing with Pre-signed URLs**

   * **Key Learning:** Understanding static resource security on the Internet. By default, S3 enables Block Public Access to protect internal data.
   * **Practical Application:** I researched and successfully deployed Pre-signed URLs. When a Client needs to view a document or image, the Backend uses AWS credentials to generate a temporary link (valid for short durations, e.g., 15–30 minutes). This technique securely shares files with authorized users without making the entire Bucket publicly accessible.

![C# S3Service Implementation of GeneratePreSignedUrl](/images/3.2-Blog2/s3service-code.jpg)

## Real-world CORS Debugging Experience

A real-world pitfall I encountered was when Frontend invoked Pre-signed URLs to upload directly to S3, browsers consistently blocked requests due to CORS (Cross-Origin Resource Sharing). While Postman testing succeeded, browser requests failed due to Same-Origin Policy.

My solution was configuring CORS Policy on the S3 Bucket, explicitly defining allowed origins (`localhost` for dev, staging domain for testing) and enabling GET/PUT methods. Through this incident, I gained a deeper understanding that Cloud security extends beyond Server-to-Server (IAM, Secret Keys) to Browser-level security.

## Conclusion

The internship not only taught me how to utilize a cloud storage service (Amazon S3), but more importantly, shaped my software design architecture mindset. Lessons in security (IAM), architectural optimization, and Cloud SDK usage form a solid foundation for my future career as a Backend / Cloud-Native Engineer.

## Reference Material

* [Amazon S3 Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
* [AWS SDK for .NET Documentation](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/welcome.html)

#AWS #AmazonS3 #CloudNative #Decoupling #AWSSDK #IAM #PreSignedURL #Backend