---
title: "Week 7 Worklog"
date: 2026-06-29
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives

* Analyze security risks and bandwidth bottlenecks associated with EC2 instances accessing Amazon S3 over the Public Internet or NAT Gateway.
* Master the architecture of Gateway VPC Endpoints (for Amazon S3 & DynamoDB), Route Table integration mechanics, and cost benefits (Free of charge).
* Practice creating a Gateway VPC Endpoint for Amazon S3 following Workshop Chapter 5.3 ("Accessing S3 from VPC").
* Update Private Subnet Route Tables to route all Amazon S3 traffic directly over AWS private backbone network.
* Verify connectivity from an isolated EC2 instance in a Private Subnet without Internet Gateway or NAT Gateway.

### Tasks Completed During the Week

| Day | Tasks | Start Date | Completion Date | Learning Resources |
| --- | --- | --- | --- | --- |
| Monday | - Explore VPC Endpoints concept and private connectivity solutions for AWS services.<br>- Compare Gateway VPC Endpoints vs Interface VPC Endpoints (AWS PrivateLink).<br>- Analyze private S3 access patterns without requiring Public IPs or NAT Gateways. | 29/06/2026 | 29/06/2026 | [VPC Endpoints Guide](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints.html)<br>[Gateway Endpoints for S3](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-s3.html) |
| Tuesday | - Set up workshop lab environment according to Chapter 5.2 ("Prerequisites").<br>- Provision test VPC, Public Subnet, Private Subnet, and an isolated EC2 instance in Private Subnet without Internet access.<br>- Test `aws s3 ls` from private EC2 instance and confirm connection timeout due to missing internet route. | 30/06/2026 | 30/06/2026 | [Workshop Prerequisites](5.2-Prerequiste/)<br>[AWS S3 CLI Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/index.html) |
| Wednesday | - Practice creating a Gateway VPC Endpoint for service `com.amazonaws.ap-southeast-1.s3` via AWS Console.<br>- Select target VPC and associate the Gateway Endpoint with the Private Subnet Route Table.<br>- Observe Route Table modifications: addition of a new prefix list route (`pl-xxxxxx`) pointing to the Endpoint ID (`vpce-xxxxxx`). | 01/07/2026 | 01/07/2026 | [Workshop S3 from VPC](5.3-S3-vpc/)<br>[Modifying Route Tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html) |
| Thursday | - **Connectivity Verification Testing:**<br>- Re-connect to the private EC2 instance and re-execute `aws s3 ls` and `aws s3 cp` commands.<br>- Verify immediate successful connection with low latency, ensuring traffic never traverses the public internet.<br>- Inspect resolved IP routes to confirm traffic routes exclusively over AWS private internal networks. | 02/07/2026 | 02/07/2026 | [Workshop S3 from VPC](5.3-S3-vpc/)<br>[Testing S3 Endpoint Connectivity](https://docs.aws.amazon.com/vpc/latest/privatelink/test-endpoints.html) |
| Friday | - Compare performance and cost efficiency of NAT Gateway vs Gateway VPC Endpoint for S3 workloads.<br>- Compile lab results, extract CLI output logs, and capture screenshot evidence for Workshop Chapter 5.3 report.<br>- Document hands-on exercises and prepare for Interface VPC Endpoint concepts. | 03/07/2026 | 03/07/2026 | [Workshop Overview](5.1-Workshop-overview/)<br>[AWS PrivateLink Pricing](https://aws.amazon.com/privatelink/pricing/) |

### Week 7 Achievements

* Clearly articulated the core differences between Gateway VPC Endpoints and Interface VPC Endpoints regarding architecture, routing, and pricing.
* Understood the routing mechanics of Gateway VPC Endpoints using Route Table Prefix Lists without altering EC2 instance IP addresses.
* Successfully built a isolated test VPC environment featuring a Private Subnet EC2 server disconnected from the Internet.
* Successfully created a Gateway VPC Endpoint for Amazon S3 and automatically attached the routing policy to the Private Subnet Route Table.
* Experimentally verified that EC2 inside Private Subnet can execute `aws s3 ls` and upload files smoothly without NAT Gateway or Internet Gateway.
* Demonstrated cost optimization advantages (saving NAT Gateway data processing charges) for high-throughput S3 data transfers.
* Compiled complete screenshot evidence and documentation covering Chapter 5.3 of the Internship Workshop Report.
