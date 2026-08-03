---
title: "Week 4 Worklog"
date: 2026-06-08
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives

* Gain an in-depth understanding of managed relational database architecture on AWS using Amazon RDS (Relational Database Service).
* Compare supported Database Engines (PostgreSQL, MySQL, MariaDB, Aurora) and select the appropriate engine based on workload requirements.
* Master Multi-AZ Deployments for high availability (HA) and Read Replicas for horizontal read scalability.
* Provision DB Subnet Groups, configure Security Groups, and deploy an Amazon RDS instance securely into Private Subnets.
* Connect a backend application to Amazon RDS, execute database migration scripts, and test Automated Backups and DB Snapshots.

### Tasks Completed During the Week

| Day | Tasks | Start Date | Completion Date | Learning Resources |
| --- | --- | --- | --- | --- |
| Monday | - Explore Amazon RDS service overview and managed database capabilities.<br>- Compare self-managed databases on EC2 vs fully managed Amazon RDS.<br>- Study DB Instance Classes, Storage Types (gp2, gp3, io1), DB Parameter Groups, and Option Groups. | 08/06/2026 | 08/06/2026 | [Amazon RDS User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html)<br>[RDS Storage Types](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Storage.html) |
| Tuesday | - Study Multi-AZ Deployments (synchronous replication) for automated failover disaster recovery.<br>- Explore Read Replicas (asynchronous replication) to offload read-heavy database workloads.<br>- Analyze enterprise-grade database infrastructure patterns on AWS. | 09/06/2026 | 09/06/2026 | [RDS High Availability (Multi-AZ)](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html)<br>[RDS Read Replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html) |
| Wednesday | - Practice creating a DB Subnet Group spanning Private Subnets across multiple Availability Zones.<br>- Configure RDS Security Group allowing inbound traffic exclusively from the EC2 Web Server Security Group (Port 5432/3306).<br>- Launch an Amazon RDS PostgreSQL instance in a Private Subnet with Public Accessibility disabled. | 10/06/2026 | 10/06/2026 | [RDS Subnet Groups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html)<br>[RDS Security Groups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.RDSSecurityGroups.html) |
| Thursday | - Practice connecting to RDS PostgreSQL from an EC2 instance in a Public Subnet using `psql` / pgAdmin.<br>- Execute SQL Migration scripts to create tables, indexes, and seed initial application data.<br>- Securely configure database connection strings using Environment Variables on the backend server. | 11/06/2026 | 11/06/2026 | [Connect to RDS Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ConnectToInstance.html)<br>[PostgreSQL on RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html) |
| Friday | - **Practice & Testing:**<br>- Learn Automated Backup mechanics (RPO/RTO) and manual DB Snapshots.<br>- Practice creating a manual DB Snapshot and restoring a new test DB Instance from the snapshot.<br>- Verify all Security Group rules to ensure no database ports are publicly exposed to the Internet.<br>- Capture screenshot evidence of RDS deployment and document findings. | 12/06/2026 | 12/06/2026 | [RDS Backups & Snapshots](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithAutomatedBackups.html)<br>[Restoring DB Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_RestoreFromSnapshot.html) |

### Week 4 Achievements

* Understood the key operational benefits of Amazon RDS over running self-managed databases on EC2 instances.
* Mastered the principles of Multi-AZ Deployments and Read Replicas for building fault-tolerant and highly scalable database layers.
* Successfully configured a multi-AZ DB Subnet Group across multiple Availability Zones.
* Deployed an Amazon RDS PostgreSQL Instance isolated inside Private Subnets with Public Access completely disabled.
* Enforced tiered security by restricting RDS Security Group inbound access solely to the EC2 Web Server Security Group on Port 5432.
* Established connectivity between the EC2 backend server and the RDS instance, successfully running schema migrations and seeding initial data.
* Executed manual DB Snapshots and verified the disaster recovery restoration process from a snapshot.
* Compiled comprehensive screenshot documentation covering RDS architecture design and security configuration.
