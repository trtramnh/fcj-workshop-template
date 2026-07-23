---
title: "Week 2 Worklog"
date: 2026-05-25
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives

* Understand the core components of Amazon VPC (IPv4 CIDR, Subnets, Route Tables, Internet Gateway).
* Differentiate between Public Subnets and Private Subnets based on routing configuration and connectivity.
* Understand the roles and operation of Route Tables, Internet Gateways, and NAT Gateways.
* Compare and distinguish Security Groups and Network ACLs in VPC network security.
* Define initial project ideas, target users, scope, and candidate AWS services for the team project.

### Tasks to Be Carried Out This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Discuss with 4 team members <br> - Brainstorm and contribute AWS project ideas <br> - Define problem statement, target users, and initial project scope <br> - Evaluate potential AWS services to be used in the project | 25/05/2026 | 25/05/2026 | [Amazon VPC Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) |
| 3 | - Learn about Amazon VPC fundamentals and IPv4 CIDR Block space allocation <br> - Explore Public Subnet, Private Subnet, Route Table, and Internet Gateway concepts <br> - Analyze network traffic flows between subnets and the Internet <br> - Draw a basic VPC network architecture diagram prior to deployment | 26/05/2026 | 26/05/2026 | [Amazon VPC Workshop](https://000003.awsstudygroup.com/) <br> [Amazon VPC Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) <br> [VPCs and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html) <br> [Route Tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html) <br> [Internet Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) |
| 4 | - Practice creating an Amazon VPC and allocating CIDR blocks for Public Subnet and Private Subnet <br> - Create and attach an Internet Gateway to the VPC <br> - Configure Route Table for Public Subnet (routing 0.0.0.0/0 to Internet Gateway) <br> - Launch EC2 instances into Public and Private Subnets to verify network connectivity | 27/05/2026 | 27/05/2026 | [Amazon VPC Workshop](https://000003.awsstudygroup.com/) <br> [Create EC2 Server](https://000003.awsstudygroup.com/4-createec2server/4.1-createec2/) |
| 5 | - Learn and compare Security Groups (instance-level, stateful) vs Network ACLs (subnet-level, stateless) <br> - Analyze Inbound Rules and Outbound Rules configuration for each security layer <br> - Practice configuring Security Groups for EC2 and Network ACLs for Subnets <br> - Test network traffic (ping, SSH, HTTP) to verify packet filtering behaviors | 28/05/2026 | 28/05/2026 | [Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html) <br> [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html) |
| 6 | - Learn the concept and operational principles of NAT Gateway <br> - Create a NAT Gateway inside the Public Subnet and allocate a static Elastic IP <br> - Update the Private Subnet Route Table to route 0.0.0.0/0 traffic to the NAT Gateway <br> - Verify that EC2 in the Private Subnet successfully accesses the Internet outbound while staying protected from inbound access <br> - Synthesize lab results and capture screenshot evidence | 29/05/2026 | 29/05/2026 | [NAT Gateway Workshop](https://000003.awsstudygroup.com/4-createec2server/4.3-natgateway/) <br> [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) |

### Week 2 Achievements

* Worked with 4 team members to establish initial project ideas, target user personas, problem scope, and identified candidate AWS core services for implementation.
* Understood the fundamental role and structure of Amazon VPC along with key components including IPv4 CIDR Blocks, Public Subnets, Private Subnets, Route Tables, and Internet Gateways.
* Completed a basic VPC architecture diagram and successfully practiced creating a VPC, provisioning Public/Private subnets, attaching an Internet Gateway, and configuring Route Tables.
* Became familiar with launching EC2 instances across different subnets to test internal network connectivity and Internet routing.
* Understood the key differences between Security Groups (stateful, instance-level) and Network ACLs (stateless, subnet-level), and practiced configuring Inbound/Outbound rules to filter network traffic.
* Learned the operational mechanics of NAT Gateway, successfully created a NAT Gateway in the Public Subnet with an Elastic IP, and updated the Private Subnet Route Table to allow outbound Internet access for Private EC2 instances.
* Synthesized practice results and captured comprehensive screenshot evidence for each VPC configuration and connectivity test step.
