---
title: "Database, Storage & Secrets"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---


In this module, we will set up the Data Storage Layer. Snaptics has mixed storage needs:
- Structured data (Transactions, Users, Spending Categories) will reside in **SQL Server**.
- Unstructured data (Invoice images, receipts) will be stored in **S3**.
- Sensitive data (DB Passwords, JWT Secrets, API Keys) will be encrypted in **Parameter Store**.
