---
title: "Blog 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# AWS LESSONS AND CLOUD-NATIVE MINDSET FROM MY INTERNSHIP

During my internship, I had the opportunity to participate in a real-world backend project where Amazon S3 was integrated for storing documents and images. Although Amazon S3 was the primary AWS service used in the project, the implementation process provided valuable opportunities to learn cloud-native application design and gain a broader understanding of the AWS ecosystem.

Beyond learning how cloud storage is integrated into backend applications, I also explored important concepts related to system architecture, security, cost optimization, and cloud best practices that are commonly applied in production environments.

### Key Lessons

Throughout the internship, I learned several important cloud-native concepts.

- **Decoupling Compute and Storage**
  - Learned how backend services can focus on business logic while uploaded files are stored in Amazon S3.
  - Understood why storing object URLs in the database is more efficient than storing binary files directly.
  - Recognized how this design improves scalability and reduces storage pressure on application servers.

- **Working with AWS SDK for Amazon S3**
  - Learned how the AWSSDK.S3 package is integrated into a .NET backend.
  - Explored common operations such as file upload, download, metadata management, and object deletion using C#.
  - Gained a better understanding of interacting with AWS services programmatically instead of relying solely on the AWS Management Console.

- **Cost Optimization**
  - Learned that unused objects can gradually increase cloud storage costs.
  - Understood the importance of cleaning up temporary files.
  - Explored Amazon S3 Lifecycle Policies for automatically removing obsolete objects.

- **IAM and Least Privilege**
  - Learned why applications should avoid using the AWS Root Account.
  - Understood how IAM users and policies provide secure access control.
  - Recognized the importance of applying the Principle of Least Privilege.

- **Protecting AWS Credentials**
  - Learned why Access Keys and Secret Keys should be stored securely using environment variables or configuration files excluded from source control.
  - Understood the risks of hardcoding sensitive credentials into application source code.

- **Secure File Sharing with Pre-signed URLs**
  - Learned how Amazon S3 Pre-signed URLs provide temporary and secure access to private objects.
  - Understood how private S3 buckets can securely share files with authorized users.

- **Understanding Browser Security**
  - Learned about Cross-Origin Resource Sharing (CORS) when frontend applications communicate with Amazon S3.
  - Understood how S3 CORS policies help allow trusted requests while maintaining security.
  - Recognized that cloud security involves both backend permissions and browser-level security policies.

### What I Learned

This internship helped me realize that cloud computing is much more than simply using AWS services. Designing cloud-native applications requires careful consideration of architecture, security, scalability, and cost optimization to build reliable systems.

Learning from a real-world project also gave me a better understanding of how AWS services work together in practice. These experiences strengthened my cloud-native mindset and provided a valuable foundation for my future development as a Backend Developer and Cloud Engineer.

### Images

<div style="display: flex; flex-wrap: wrap; justify-content: center; gap: 24px; margin-top: 20px;">

  <div style="width: 420px; text-align: center;">
    <img src="/fcj-workshop-template/images/3-BlogsPosted/3.3-Blog3/blog3.jpg"
         alt="Amazon S3 Architecture"
         style="width:100%; height:260px; object-fit:contain; background:#fafafa; border-radius:10px; box-shadow:0 2px 8px rgba(0,0,0,0.15);">
    <p>Cloud-native architecture integrating Amazon S3 into the backend application.</p>
  </div>

  <div style="width: 420px; text-align: center;">
    <img src="/fcj-workshop-template/images/3-BlogsPosted/3.3-Blog3/blog3.1.jpg"
         alt="Amazon S3 Pre-signed URL"
         style="width:100%; height:260px; object-fit:contain; background:#fafafa; border-radius:10px; box-shadow:0 2px 8px rgba(0,0,0,0.15);">
    <p>Generating Amazon S3 Pre-signed URLs using the AWS SDK for .NET.</p>
  </div>

</div>

### Reference Material

The knowledge summarized in this blog was learned through:
* [Amazon S3 Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
* [AWS SDK for .NET Documentation](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/welcome.html)