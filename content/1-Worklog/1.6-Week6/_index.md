---
title: "Week 6 Worklog"
date: 2026-06-22
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives

* Understand Infrastructure as Code (IaC) principles and the benefits of AWS CloudFormation for automated provisioning.
* Master CloudFormation template syntax (YAML/JSON) including Parameters, Resources, Outputs, and Mappings sections.
* Master resource automation scripting using AWS CLI combined with JMESPath `--query` filters and JSON formatting.
* Explore CloudFormation Stack Lifecycle management, Stack Updates, Rollback behaviors, and Drift Detection.
* Practice deploying a complete 2-tier infrastructure (VPC, Subnets, Security Groups, EC2, RDS) using a single CloudFormation template via AWS CLI.

### Tasks Completed During the Week

| Day | Tasks | Start Date | Completion Date | Learning Resources |
| --- | --- | --- | --- | --- |
| Monday | - Explore Infrastructure as Code (IaC) fundamentals and AWS CloudFormation overview.<br>- Compare manual management via Console vs automated provisioning with CloudFormation templates.<br>- Study core YAML template sections: AWSTemplateFormatVersion, Description, Parameters, Resources, and Outputs. | 22/06/2026 | 22/06/2026 | [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)<br>[Template Anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html) |
| Tuesday | - Practice writing a CloudFormation YAML template provisioning basic VPC networking infrastructure.<br>- Declare resources: AWS::EC2::VPC, AWS::EC2::Subnet, AWS::EC2::InternetGateway, AWS::EC2::RouteTable, and SubnetRouteTableAssociation.<br>- Utilize Intrinsic Functions (`!Ref`, `!Sub`, `!GetAtt`) to resolve resource dependencies dynamically. | 23/06/2026 | 23/06/2026 | [CloudFormation Resource Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)<br>[Intrinsic Functions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html) |
| Wednesday | - Enhance automation scripting skills using AWS CLI in Linux Bash shell environment.<br>- Utilize `--query` options with JMESPath syntax to extract dynamic resource properties (VPC ID, Instance IP).<br>- Write shell scripts to inspect AWS resource states and output formatted JSON reports. | 24/06/2026 | 24/06/2026 | [AWS CLI Command Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/index.html)<br>[Filtering AWS CLI Output](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-filter.html) |
| Thursday | - Expand CloudFormation template to include Security Groups, EC2 Instance, and RDS DBInstance resources.<br>- Study CloudFormation Stack Events and automatic Rollback mechanisms on resource creation failures.<br>- Learn how to use Drift Detection to identify manual configuration drift against the declared template code. | 25/06/2026 | 25/06/2026 | [Stack Management](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacks.html)<br>[Detecting Drift](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-stack-drift.html) |
| Friday | - **Practice & Automation:**<br>- Execute `aws cloudformation create-stack` to launch the complete 2-tier stack via CLI.<br>- Track stack creation progress using `aws cloudformation describe-stack-events`.<br>- Verify successful stack creation and retrieve application endpoint URL from Outputs.<br>- Save YAML code to repository and document execution evidence. | 26/06/2026 | 26/06/2026 | [AWS CLI CloudFormation](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudformation/index.html)<br>[Deploying Stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stacks.html) |

### Week 6 Achievements

* Gained a solid grasp of Infrastructure as Code (IaC) principles, appreciating reusability, consistency, and error reduction.
* Mastered YAML syntax for CloudFormation templates and effective use of Intrinsic Functions (`!Ref`, `!Sub`, `!GetAtt`).
* Authored a production-ready CloudFormation template defining VPC, Subnets, Route Tables, Internet Gateway, Security Groups, EC2, and RDS.
* Mastered CLI data extraction using AWS CLI with JMESPath queries (`--query`) for operational automation.
* Deepened understanding of Stack Lifecycle management, automated rollbacks, and configuration Drift Detection.
* Successfully launched a full 2-tier cloud environment via a single `aws cloudformation create-stack` command.
* Committed `template.yaml` to the workspace repository and compiled screenshot logs verifying stack execution.
