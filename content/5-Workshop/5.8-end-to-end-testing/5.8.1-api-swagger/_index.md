---
title: "API Testing (Swagger)"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.8.1. </b> "
---


If you configure the environment variable `ASPNETCORE_ENVIRONMENT = Development` on ECS, the Swagger UI interface will be enabled by default.

Access the browser via the ALB DNS:
`http://<ALB_DNS_NAME>/swagger`

### Actual Testing Steps:

**1. Registration and Login (Auth)**
- Find the Endpoint `POST /api/Auth/register`, enter a body to create a new account.
- Find the Endpoint `POST /api/Auth/login`, the system will call down to SQL Server (RDS), authenticate the password, get the JWT Key from Parameter Store, and return a `token` string.
- Click the green padlock **Authorize** button at the top of Swagger, enter `Bearer <TOKEN>` to authenticate subsequent requests.

**2. Test Invoice Upload & AI**
- Find the Endpoint `POST /api/Transactions/scan-receipt`.
- Choose an invoice image file from your computer and execute.
- **Verify the results:**
  1. Open AWS Console, go to **S3**, you will see the invoice image is now in the `s3-bucket-snaptics` bucket.
  2. Open **SQS**, you will see 1 Message pop up (If the backend is configured to push through the queue instead of a direct call).
  3. Open the Swagger screen, you will receive a JSON string returning detailed price (Total) and store name (Merchant) accurately extracted by Azure Document Intelligence!
