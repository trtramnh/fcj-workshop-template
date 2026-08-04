---
title: "Amazon S3 for Uploads"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---



<!-- TODO: Insert screenshot of Amazon S3 Bucket creation/list here -->
![S3 Bucket](/images/5-Workshop/placeholder-s3.png)

Whenever a Snaptics user uploads an invoice for AI analysis, that image is not saved in the Database or the Container's hard drive; it is pushed to S3.

### 1. Create S3 Bucket
- Open **Amazon S3 ➔ Create bucket**.
- **Bucket name:** `s3-bucket-snaptics-123` (Add some random numbers because S3 names must be globally unique).
- **Region:** `ap-southeast-1`.
- **Block Public Access settings for this bucket:** Ensure **Block all public access** is selected (Turned on). We want to keep the files strictly secure.

### 2. CORS (Cross-Origin Resource Sharing) Configuration
If you plan to build a Frontend Web (like React/Vue) that calls S3 directly via a *Pre-signed URL* issued by the API, you must enable CORS.
- Go to the newly created Bucket ➔ **Permissions** tab.
- Scroll down to the **Cross-origin resource sharing (CORS)** section ➔ Click **Edit** and paste the following JSON:

```json
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "PUT",
            "POST"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": []
    }
]
```
*(In reality, `AllowedOrigins` should be changed to your frontend domain instead of `*`).*
