---
title: "Messaging & AI"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---


The core strength of Snaptics lies in its asynchronous processing capabilities. When a user uploads an invoice, the system needs to scan it with OCR, classify it with AI, and store it. Instead of making the user wait with a loading spinner on the UI, Snaptics publishes that task to a Queue, informs the user "Processing", and executes it in the background via Hangfire and SQS.
