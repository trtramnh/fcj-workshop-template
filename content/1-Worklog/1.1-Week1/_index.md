---
title: "Week 1 Worklog"
date: 2026-05-18
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Week 1 Objectives:

* Become familiar with the First Cloud Journey program and team members.
* Read and understand the rules and regulations of the internship unit.
* Gain an overview of Cloud Computing and core AWS service categories (Compute, Storage, Networking, and Database).
* Learn how to set up and secure an AWS Free Tier account, install AWS CLI, and understand fundamental Amazon EC2 components.
* Find team to build project using AWS.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Get acquainted with FCAJ members <br> - Read and note down internship unit rules and regulations | 18/05/2026 | 18/05/2026 | [FCAJ Regulations](https://hcm-rules.awsfcaj.com/1-regulations/) |
| 3 | - Learn about Cloud Computing and AWS overview <br> - Explore core AWS service categories: <br>&emsp; + Compute <br>&emsp; + Storage <br>&emsp; + Networking <br>&emsp; + Database | 19/05/2026 | 19/05/2026 | [AWS Cloud Journey](https://cloudjourney.awsstudygroup.com/) <br> [AWS Overview](https://aws.amazon.com/what-is-aws/) |
| 4 | - Create and secure an AWS Free Tier account (MFA setup & billing check) <br> - Become familiar with AWS Management Console <br> - Install and configure AWS CLI default profile | 20/05/2026 | 20/05/2026 | [AWS Cloud Journey](https://cloudjourney.awsstudygroup.com/) <br> [AWS Free Tier](https://aws.amazon.com/free/) <br> [AWS CLI User Guide](https://docs.aws.amazon.com/cli/) |
| 5 | - Learn core concepts of Amazon EC2: <br>&emsp; + AMI <br>&emsp; + Instance Types <br>&emsp; + EBS <br>&emsp; + Security Group <br>&emsp; + Key Pair <br>&emsp; + Elastic IP | 21/05/2026 | 21/05/2026 | [AWS Cloud Journey](https://cloudjourney.awsstudygroup.com/) <br> [Amazon EC2 Guide](https://docs.aws.amazon.com/ec2/) |
| 6 | - **Practice:** <br>&emsp; + Launch an Amazon EC2 instance <br>&emsp; + Connect to EC2 via SSH <br>&emsp; + Create, attach, and verify an EBS volume | 22/05/2026 | 22/05/2026 | [AWS Cloud Journey](https://cloudjourney.awsstudygroup.com/) <br> [Amazon EBS User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html) |

### Week 1 Achievements:

* Gained a clear understanding of Cloud Computing concepts and main AWS service categories:
  * Compute
  * Storage
  * Networking
  * Database

* Successfully created and secured an AWS Free Tier account, enabling Multi-Factor Authentication (MFA) and checking billing settings.

* Became familiar with navigating the AWS Management Console to locate and view essential services.

* Installed AWS CLI on local machine and successfully configured the default profile (`Access Key`, `Secret Key`, `Default Region`, and `Output Format`).

* Verified AWS CLI setup using foundational commands:
  ```bash
  aws --version
  aws configure
  aws configure list
  aws sts get-caller-identity
  aws ec2 describe-regions
  ```

* Learned how to launch an Amazon EC2 instance using the AWS Console.

* Successfully connected to the EC2 instance using SSH with a Key Pair.

* Successfully created, attached, and verified an Amazon EBS volume on the EC2 instance.

* Gained a basic understanding of the fundamental roles of AMI, Security Group, Key Pair, and Elastic IP in EC2 instance management.

* Found a team with 4 participants and ready to build project using AWS.