---
title: "Week 9 Worklog"
date: 2026-07-13
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives

* Analyze detailed project requirements for the team proposal "IoT Weather Platform for Lab Research" tailored for the ITea Lab research team.
* Define project scope and goals (Ingesting telemetry from 5 Raspberry Pi + ESP32 weather stations, expandable to 15 stations, MQTT protocol).
* Select and design a unified AWS Serverless solution: AWS IoT Core, AWS Lambda, Amazon S3 Data Lake, AWS Glue, API Gateway, AWS Amplify, and Amazon Cognito.
* Draw comprehensive architecture diagrams covering Edge Architecture (Edge Devices) and Platform Architecture (Cloud Backend).
* Calculate monthly operational expenditure using AWS Pricing Calculator and establish a risk management matrix for the proposal.

### Tasks Completed During the Week

| Day | Tasks | Start Date | Completion Date | Learning Resources |
| --- | --- | --- | --- | --- |
| Monday | - Convene team meeting to finalize the Proposal for "IoT Weather Platform for Lab Research".<br>- Analyze existing limitations: legacy weather stations required manual data collection, lacking centralized storage and real-time analytics.<br>- Define target personas (5 ITea Lab researchers) and core problem statements. | 13/07/2026 | 13/07/2026 | [Proposal Document](2-Proposal/)<br>[AWS IoT Core Overview](https://docs.aws.amazon.com/iot/latest/developerguide/what-is-aws-iot.html) |
| Tuesday | - Design telemetry data ingestion and ETL transformation pipelines.<br>- Select AWS IoT Core to ingest MQTT messages from Raspberry Pi edge devices.<br>- Design S3 Data Lake layout: Bucket 1 for raw telemetry (Raw Data Lake), Bucket 2 for transformed analytical datasets. | 14/07/2026 | 14/07/2026 | [AWS IoT Rules](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rules.html)<br>[Building Data Lakes on AWS](https://aws.amazon.com/solutions/implementations/data-lake-solution/) |
| Wednesday | - Research automated analytics solutions using AWS Glue (Crawlers for S3 cataloging & ETL Jobs for data partitioning).<br>- Evaluate AWS Amplify for hosting Next.js web dashboards and Amazon Cognito for secure user authentication.<br>- Select Infrastructure as Code tools (AWS CDK/SDK) for backend deployment. | 15/07/2026 | 15/07/2026 | [AWS Glue Developer Guide](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html)<br>[Amazon Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html) |
| Thursday | - **Architecture Diagram Drafting:**<br>- Draw Edge Architecture depicting sensor streams: ESP32 -> Raspberry Pi (Docker) -> MQTT over Wi-Fi.<br>- Draw Platform Architecture mapping interactions across IoT Core -> S3 -> Glue -> Lambda -> API Gateway -> Amplify Next.js.<br>- Formulate risk matrix (network loss, sensor failure, budget overrun) and fallback mitigations. | 16/07/2026 | 16/07/2026 | [AWS Architecture Icons](https://aws.amazon.com/architecture/icons/)<br>[Proposal Section 3](2-Proposal/#3-kiến-trúc-giải-pháp) |
| Friday | - **Budget Estimation & Proposal Finalization:**<br>- Utilize AWS Pricing Calculator to compute detailed service breakdowns (Lambda $0, S3 $0.15, Amplify $0.35, Glue $0.09, IoT Core $0.08).<br>- Confirm optimized monthly cloud cost estimate: **$0.70/month** (~$8.40/year).<br>- Integrate proposal documentation into Hugo structure (`content/2-Proposal/`) and verify site rendering. | 17/07/2026 | 17/07/2026 | [AWS Pricing Calculator](https://calculator.aws/)<br>[Proposal Budget Section](2-Proposal/#6-ước-tính-ngân-sách) |

### Week 9 Achievements

* Successfully formulated the "IoT Weather Platform for Lab Research" project proposal, solving legacy manual weather monitoring limitations.
* Designed a full serverless cloud architecture integrating AWS IoT Core, S3 Data Lake, AWS Glue, Lambda, API Gateway, Amplify, and Cognito.
* Produced standardized architecture diagrams covering both Edge Architecture (Raspberry Pi/ESP32) and Platform Architecture (AWS Serverless Cloud).
* Developed a structured risk mitigation matrix addressing hardware/network failures with local Docker container buffering strategies.
* Computed exact monthly infrastructure costs using AWS Pricing Calculator, achieving extreme cost efficiency ($0.70/month).
* Published complete bilingual Proposal content into the Hugo platform (`content/2-Proposal/_index.vi.md` and `_index.md`).
