---
title: "Event 1"
date: 2026-08-01
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Agent Forge – Deep Dive Day 1: Foundations & Agent Setup

---

### Event Purpose

The event was organized to provide foundational knowledge and deep-dive technical guidance on building, deploying, and operating **Agentic AI** systems in production environments.

Through the introduction of **Amazon Bedrock AgentCore**, participants learned about key components required to transition AI Agents from experimental POCs to production readiness, including **Runtime**, **Gateway**, **Identity**, and tool integration mechanisms.

---

### Speakers

- **Nghia Tran** – Agentic Solutions Architect (SA)
- **Anh Pham** – Cloud Consultant, G-AsiaPacific Vietnam

---

### Key Highlights

#### 1. Agentic AI Overview
* Conceptual introduction to AI Agents, agent autonomy, and the difference between hard-coded workflows and systems capable of self-planning, tool selection, and multi-step execution.
* Introduction to **Strands Agents SDK**, an open-source model-driven framework for building AI Agents with reduced boilerplate coordination code.

#### 2. Amazon Bedrock AgentCore Runtime
* AgentCore Runtime provides a dedicated serverless environment for running AI Agents.
* Each session runs in an isolated **microVM**, guaranteeing CPU, memory, and filesystem isolation across users.
* Runtime supports session state persistence, asynchronous execution, and long-running tasks.

#### 3. Amazon Bedrock AgentCore Identity
* **Inbound authentication**: Controls which users or applications are authorized to invoke the agent.
* **Outbound authentication**: Controls how the agent accesses external APIs, AWS services, or third-party platforms.
* Agents are assigned distinct *workload identities* and use *workload access tokens* for scoped resource access. Flexible integration with **Amazon Cognito** and OAuth/JWT Identity Providers.

#### 4. Amazon Bedrock AgentCore Gateway
* Centralized, secure access point connecting AI Agents to tools, APIs, Lambda functions, or other agents.
* Converts existing APIs into **Model Context Protocol (MCP)** compliant tools, allowing standardized tool discovery and invocation.
* Native **semantic search** capabilities to discover tools via natural language queries when dealing with large API registries.

#### 5. Hands-on Lab
* Deployed a foundational AI Agent onto AgentCore Runtime.
* Connected the agent to external tools and Knowledge Bases.
* Built a web interface for direct user interaction with the agent.
* Integrated **Amazon Cognito** for user authentication prior to granting agent access.

---

### Key Learnings

#### Technical Domain Knowledge
* **Production-Ready Agentic Architecture**: Building production AI Agents requires a complete framework including session isolation (microVMs), bi-directional authentication (Identity), centralized tool management (Gateway), and robust observability.
* **Model-Driven Development**: Leveraging Strands SDK and MCP Servers standardizes external tool invocation over complex custom orchestration code.

#### Security & Governance
* **Secure Connections & Data**: Minimize public internet transit via **AWS PrivateLink**, encrypt end-to-end data flows, and enforce Least Privilege access controls.

---

### Practical Application to Snaptics Project

#### 1. Dedicated AI Agent for Financial Insights
Rather than one-off model invocations, Snaptics can deploy an agent to:
* Analyze user transactions, wallets, and budgets.
* Evaluate spending patterns and query relevant backend APIs automatically.
* Generate situational financial advice and push alerts to the Notification system.
* Align with current architecture: Triggering `/AiAssistant/insight` backend endpoint post-transaction confirmation.

#### 2. API Tooling via AgentCore Gateway (MCP)
* Encapsulate transaction, budget, wallet, savings goals, and notification APIs into modular MCP tools.
* AgentCore Gateway acts as a governance layer, ensuring the AI agent only invokes authorized tools without direct DB access.

#### 3. Protecting Financial User Data
* Combine AgentCore Identity with Amazon Cognito to enforce strict multi-tenant isolation.
* Granular scoping of tool capabilities per user role.

#### 4. Human-in-the-Loop Implementation
For sensitive operations (budget modifications, transaction deletion/edits, savings target changes):
* AI generates recommendations requiring explicit user confirmation prior to execution.
* Balances AI automation with user authority and data protection.

---

### Workshop Experience

An intensive, highly practical workshop pairing architectural deep-dives with hands-on labs. The lab exercises reinforced the end-to-end pipeline from simple agent setup to production deployment readiness.

---

### Key Takeaways

* Reasoning capability alone is insufficient for a production-ready AI Agent. Systems require well-defined control layers across Runtime, Identity, Gateway, permissions, and data boundary isolation.
* In Snaptics, AI must not be granted direct, unconstrained access to financial stores. Every capability should be exposed as an authenticated, scoped tool requiring user confirmation for sensitive actions.

---

### Event Photos

Below are photos captured during the **AWS FCAJ Agent Forge – Deep Dive Day 1** event, featuring speaker sessions, hands-on labs, and networking with AWS community members.

<div style="display: flex; gap: 15px; justify-content: center; align-items: flex-start; flex-wrap: wrap; margin-top: 20px;">
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.1-Event1/event1_1.jpg" alt="Event Venue at Bitexco Financial Tower" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Figure 1:</b> Event venue at Bitexco Financial Tower.</p>
  </div>
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.1-Event1/event1_2.jpg" alt="Speaker presentation on Agentic AI & AgentCore" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Figure 2:</b> Keynote presentation on Agentic AI & AgentCore.</p>
  </div>
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.1-Event1/event1_3.jpg" alt="Me and my team members at the Agent Forge – Deep Dive event" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Figure 3:</b> Me and my team members at the Agent Forge – Deep Dive event.</p>
  </div>
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.1-Event1/event1_4.jpg" alt="Group photo with speakers and participants" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Figure 4:</b> Commemorative photo with speakers and participants.</p>
  </div>
</div>

---

> **Summary:** AWS FCAJ Agent Forge – Deep Dive Day 1 delivered a comprehensive framework for building and operating AI Agents on AWS. Most importantly, it helped shape the AI roadmap for Snaptics: moving beyond simple LLM prompts toward a secure, tool-connected, asynchronous, and user-guarded AI architecture.
