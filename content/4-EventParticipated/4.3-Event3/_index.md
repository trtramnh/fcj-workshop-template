---
title: "Event 3"
date: 2026-07-25
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# FCAJ – Agentic AI Build Week Sharing Session

---

### Event Purpose

The **FCAJ – Agentic AI Build Week Sharing Session** was organized to provide practical insights from participating teams in the FCAJ Agentic AI Build Week. 

The event focused on explaining how Agentic AI applications were planned, implemented, and deployed on AWS during the buildathon. It introduced cloud-native architectures, key technical decisions, and lessons learned about teamwork, problem-solving, and software development under strict time constraints.

---

### Speakers

- **International Guest Speaker** – Industry Technology Leader & Cloud/AI Expert
- **One Team** – FCAJ Build Week Participant Team
- **Plan V** – FCAJ Build Week Participant Team
- **3KA** – FCAJ Build Week Participant Team
- **Dream AI Team** – FCAJ Build Week Participant Team
- **Six Pillar Team** – FCAJ Build Week Participant Team

---

### Key Highlights

#### 1. Opening Keynote & Industry Trends
* Opening address by an international guest speaker sharing career experiences in the technology industry.
* Highlighted the exponential growth of Artificial Intelligence and its transformative impact on modern software development.
* Emphasized that developers and professionals should maintain strong curiosity, embrace unfamiliar technologies, and foster continuous learning rather than focusing solely on static technical skills.
* Highlighted adaptability and self-improvement as essential traits for careers in Cloud Computing and AI.

#### 2. Build Week Development Journey & Real-World Use Cases
* Participating teams presented their complete development lifecycles during the buildathon, covering problem identification, solution rationale, system architecture, technical hurdles, and post-competition takeaways.
* **Intelligent Self-Service Food-Ordering System**: An AI-supported kiosk allowing customers to place orders through conversational AI interactions.
* **AI in Financial Services**: Automated customer communication, transaction processing, and financial insight generation.

#### 3. Deep Dive into Cloud Architectures & AWS Managed Services
* Walkthrough of cloud architectures demonstrating how AWS managed services were combined to support reasoning, backend processing, storage, application delivery, security, and observability:
  * **Amazon Bedrock & Amazon Bedrock AgentCore**: Used for AI reasoning, agent orchestration, and Generative AI capabilities.
  * **Amazon API Gateway & AWS Lambda**: Built serverless backend microservices and API endpoints.
  * **Amazon SQS**: Handled asynchronous communication and background task processing.
  * **Amazon DynamoDB & Amazon S3**: Stored structured application data and unstructured media/receipt files.
  * **Amazon OpenSearch Service**: Enabled semantic search and vector-based retrieval for RAG pipelines.
  * **Amazon ECS, AWS Fargate & Amazon ECR**: Built, containerized, and managed scalable container applications.
  * **Amazon CloudFront, Amazon Cognito & Application Load Balancer (ALB)**: Delivered secure, low-latency, and authenticated web application access.
  * **Amazon CloudWatch & AWS CloudTrail**: System logging, real-time performance monitoring, and audit tracking.
  * **AWS WAF, GuardDuty, IAM, KMS & Secrets Manager**: Protected web applications, managed identity access, encrypted data at rest/in transit, and secured API credentials.

---

### Key Learnings

#### Technical Knowledge & Cloud Architecture
* **Cloud-Native AI System Design**: Gained deep insights into combining multiple AWS managed services to build scalable, resilient Agentic AI systems rather than relying solely on raw foundation model APIs.
* **End-to-End Infrastructure Integration**: Understood the necessity of combining serverless compute, containerization, vector storage, API gateways, and robust security controls for enterprise-grade deployment.

#### Practical Learning & Engineering Execution
* **Architecture Planning Under Pressure**: Learned the value of designing system architecture prior to coding, prioritizing scalability, security, monitoring, and maintainability from day one.
* **Overcoming Development Hurdles**: Observed how teams managed changing requirements, API integrations, and tight buildathon timelines through clear task decomposition.

#### Professional Growth & Continuous Improvement
* Strengthened understanding of teamwork, cross-functional collaboration, and iterative software delivery.
* Reaffirmed the importance of continuous learning and hands-on experimentation in cloud-native technologies.

---

### Practical Application to Snaptics Project

#### 1. Serverless Agentic Backend with Bedrock AgentCore & AWS Lambda
* Integrate **Amazon Bedrock AgentCore** and **AWS Lambda** via **Amazon API Gateway** into **Snaptics** to power an autonomous financial advisory agent.
* Enable the agent to process complex user financial queries, trigger backend microservices, and provide contextual budgeting advice.

#### 2. Asynchronous Task Processing & Receipt Scanning Pipeline
* Utilize **Amazon SQS** and **AWS Lambda** for background processing of receipt OCR parsing and transaction batch updates, ensuring responsive UI performance without blocking user interactions.

#### 3. Structured Data & Vector-Based RAG Architecture
* Store structured user transaction records in **Amazon DynamoDB** and unstructured receipt images in **Amazon S3**.
* Implement **Amazon OpenSearch Service** to index financial guidelines and spending logs, providing retrieval-augmented generation (RAG) for personalized financial recommendations.

#### 4. Enterprise Security, Secret Management & Monitoring
* Protect **Snaptics** API endpoints using **AWS WAF**, **Amazon Cognito**, and **AWS Identity and Access Management (IAM)**.
* Secure database credentials and external API tokens via **AWS Secrets Manager** and **AWS KMS**.
* Monitor system health, API latency, and audit security events using **Amazon CloudWatch** and **AWS CloudTrail**.

---

### Event Experience

Attending the **FCAJ – Agentic AI Build Week Sharing Session** at Bitexco Financial Tower provided a practical perspective on taking an AI project from initial concept to a deployed cloud solution. 

Hearing team stories about architectural choices, trade-offs, and debugging under pressure demonstrated that building production AI solutions requires holistic system design, secure infrastructure, and strong team coordination.

---

### Key Takeaways

* Developing an Agentic AI solution extends far beyond connecting to an LLM; it requires robust compute infrastructure, serverless pipelines, vector search, security controls, and operational observability.
* Combining **Amazon Bedrock AgentCore**, **AWS Lambda**, **Amazon ECS/Fargate**, **DynamoDB**, and **OpenSearch Service** creates a highly scalable blueprint applicable to complex domain platforms like **Snaptics**.
* Prioritizing architecture planning, security best practices, and iterative development is essential for success in fast-paced software projects.

---

### Event Photos

Below are actual photos captured during the **FCAJ – Agentic AI Build Week Sharing Session** at Bitexco Financial Tower, showcasing the architecture slides, keynote presentation, venue setup, and commemorative check-in.

<div style="display: flex; gap: 15px; justify-content: center; align-items: flex-start; flex-wrap: wrap; margin-top: 20px;">
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.3-Event3/event3_1_workflow.jpg" alt="AI-Native Application Workflow Diagram" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Figure 1:</b> AI-Native Application & Retrieval Services Workflow (Knowledge Base, Bedrock Model, Draw.io MCP & AWS Pricing MCP).</p>
  </div>
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.3-Event3/event3_2_architecture.jpg" alt="Cost-Efficient Agentic Cloud Architecture" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Figure 2:</b> Cost-efficient Agentic Cloud Architecture presented by a Build Week team (Route53, API Gateway, AgentCore Runtime, Subagents & Observability).</p>
  </div>
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.3-Event3/event3_4_opening.jpg" alt="Opening Keynote Session by Guest Speaker" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Figure 3:</b> Opening presentation of FCAJ – Agentic AI Build Week by the international guest speaker.</p>
  </div>
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.3-Event3/event3_5_venue.jpg" alt="Event Venue Setup at Bitexco Financial Tower" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Figure 4:</b> Participants and event venue setup at Bitexco Financial Tower.</p>
  </div>
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.3-Event3/event3_3_me.jpg" alt="Attendee photo at AWS First Cloud Journey AI wall" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Figure 5:</b> Attending the FCAJ Agentic AI event at AWS Vietnam office / Bitexco Financial Tower.</p>
  </div>
</div>

---

> **Summary:** The FCAJ – Agentic AI Build Week Sharing Session delivered practical knowledge on planning, building, deploying, and securing real-world AI systems on AWS. The event highlighted cloud-native architecture patterns, serverless/container workflows, security standards, and teamwork principles that provide a strong foundation for future cloud and AI projects like Snaptics.
