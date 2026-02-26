---
title: "AWS Data & Generative AI Partner Builder"
tags: [event, workshop, aws, genai, presales]
created: 2026-02-26
---

# Schedule

Timings (Duration) Session Title Sub-topics

09:00 - 10:00 Operationalizing GenAI Apps in Production
GenAI Path to Production, MLOps / FMOps / GenAIOps


# Gen AI Strategy - From Ideation (POC) to Production
- AI Agent (task-oriented) vs Agentic AI (goal-oriented) from an agent dedicated to one task to help with the human to the system that running multiple agents to achive the tasks
## Gen AI Application life cycle
- ideation (from customer) 
- value model
- data strategy (what kind of data, text/voice/video, structure/unstructure)
- POC (MVP to Minimum Lovable Product)
- Evaluate (are they work fine, what could be done to fix)
- optimize (feature engineer, prompt tuning, finetuning)
- deploy(need to implement feedback loop from deploy back to data strategy), scale (all about security and governance back to data strategy again, as well as responsible ai).

## Ideation
Picking a use case

Singapore
- financial, insurance, banking
- retail and ecommerce
- public sector
Indonesia
- banking
- healthcare
- manufacturing
Vietnam
- manufacturing
Thailand
- insurance
- healthcare

## Value model
Total cost of ownership
1. model api costs 25-30% of the total cost, 
- never choose the greatest and latest model but the best model for your use case
2. Model Optimization 15%
3. Infrastructure & DevOps 10%
4. Data Strategy 15%
5. Talent & Development 20%
6. Operations & Support 10%
7. Ethical AI 5%
+ Enhance Customer Experience
+ Boost Employee Productivity
+ Optimize Business Operation (Every solution should have the business impact)
Return on Investment (ROI)

## Business Value
make assumption for cost of ownership then roi
- how many queries, cost
- how much money we are saving

# Builidng
Data
- data is your differentiator
- data sources 
- most part of GenAI come from data and analytics, focus on end-to-end data architecture
GenAI
- Amazon Bedrock are marketplace of LLMs
- Agents get user input, decompose into steps, execute action
- Start small and focused, don't bite more that you can chewed.
- Once the v1 works, move to v2/v3/v4. from static query to dynamic query to analysis to enterprise action

## Evaluate
From automatic eval to manual eval, help evaluate Accuracy (F1/ROGUE/BLEU), Cost, Speed (need to balance between cost and latency)
### Bedrock Model Evaluation 
- Programmatic Evaluation : Accurary, Robustness, Toxicity
- LLM as a Judge : Correctness, Completeness, Helpfulness, Relevance, Coherence, Readability
- Human Evaluation : Creativity, Style, Tone, Accurary, Consistency, Brand Voice
- Retrieval : Context Coverage, Context Relevance
- Retrieval & Generation : Correctness, Completeness, Helpfulness, Citation Precision, Citation Coverage, Logical Coherance, Faithfulness
- Responsible AI : Harmfulness, Steoreotyping, REfusal
- Custom Metrics 

## Optimize
1. Unify customer experience (Supervisory agent)
- Banking, Telco are moving towards this trends

## Deploy
CICD, Monitoring, Logging

## Security, Governance and Responsible AI
Defense in Depth for generative AI
- Government Data and AI Regulation
- Responsible AI Dimenisons : Fairness, Explainability, Controlability, Safety, Privacy & Security, Governance, Transparency, Veracity & Robustness

Amazon Bedrock Guardrails : Policies for
- Prompt Attack
- Denied Topics
- Content Filters
- PII Redaction
- Word Filter
- Contextual Grounding

## Result and Scale
- Before AI and After AI Implementation

10:00 - 10:45 Nova Foundation Models - Deep Dive
Nova Micro / Lite / Canvas / Reel

10: 45 - 11:00 Break

11:00 - 12:30 Workshop - Amazon Nova multimodal understanding workshop
Customization & Batch Inference, Guardrails, Evaluation & Bedrock Knowledge BasesBedrock Agents

## [Data & AI PB] Amazon Nova multimodal understanding workshop
```
# Workshop temporary credentials (redacted)
export AWS_DEFAULT_REGION="us-west-2"
export AWS_ACCESS_KEY_ID="<REDACTED>"
export AWS_SECRET_ACCESS_KEY="<REDACTED>"
export AWS_SESSION_TOKEN="<REDACTED>"
```
`https://github.com/aws-samples/amazon-nova-samples/tree/main/customization/SageMakerTrainingJobs/getting_started`

12:30 - 13:30 Lunch Break

13: 30 - 14:30 Agentic AI on AWS, AI Agents Security
Bedrock Agents, AgentCore, Kiro, Strands SDK, Q-Agents

## Building an agent
Observability in the Gen AI system is very complex, normally cloud watch gain CPU/mem/etc. but we might watch is our agent drift from the current task, etc.
## AgentCore
- Time to value
Take out the infrastructure pain, reduce time to production
- Flexible
Can choose any model or framework
- Trusted
Security as the first principle
### 7 services Under the hood
- AgentCore Runtime : Secure and Isolated containerized environment to run your agent and endpoint from any models and any frameworks with Runtime decorator, Observability config, Indentity config
- AgentCore Gateway : convert APIs, Lambda function into MCP-compatible tools, access thousands of tools. The Gateway will call API Endpoint/AWS Lambda Target to the tools, APIs and resources
- AgentCore Identity : secure delegated access for AI Agents that probide access control and permission for inbound Auth and outbound Auth
- AgentCore Memory : Enterprise grade data privacy for each agent across context and session, customize memory pattern based on use case (Redis for short-term: Chat Message, Session State/RDS for Long-term: Semantic, User Preferences, Summary), Automatic extraction module
- AgentCore Browser : low latency browser session for real-time monitoring, playwright compatible
- AgentCore Code Interpreter : secure write and execute code in sandbox environment
- AgentCore Observability : integrate with CloudWatch and 3rd party in OpenTelemetry format (OTEL)

## AWS AI Stack
- AWS Trainium, Inferentia
- AWS SageMaker AI
- AWS Bedrock

## AWS Marketplace
- AI Agents and tools

14:30 - 16:30 Workshop - Building Agentic Workflows on AWS
Agentic workflow with Strands, Agentic workflow with Sagemaker, AI Vibe coding with Kiro

```
# Workshop temporary credentials (redacted)
export AWS_DEFAULT_REGION="us-west-2"
export AWS_ACCESS_KEY_ID="<REDACTED>"
export AWS_SECRET_ACCESS_KEY="<REDACTED>"
export AWS_SESSION_TOKEN="<REDACTED>"
```
```
AgentCoreRoleArn
<REDACTED_ARN>
agent-workshop-cfn
ARN of the IAM role for Strands Agents workshop
output
AgentCoreRoleName
agentcore-agent-role
agent-workshop-cfn
Name of the IAM role for Strands Agents workshop
output
Password
<REDACTED>
agent-workshop-cfn
Code-server Password
<REDACTED>
RDPAddress
<REDACTED>
agent-workshop-cfn
RDP connection address (IP:port)
output
RDPPassword
<REDACTED>
agent-workshop-cfn
Password for RDP access
output
RDPUsername
<REDACTED>
agent-workshop-cfn
Username for RDP access
output
URL
https://<REDACTED>
agent-workshop-cfn
Code-server URL
output
```

16:30 - 17:30 Bedrock v/s Sagemaker AI
Bedrock Cost Optimization, Quiz + Wrap-up, pre-trained v/s custom models, cost optimization techniques

## Generative AI Journey and AWS Generative AI Stack
- Understand customers on their Role, Jobs to be done, AWS Services needed
- SageMaker AI and Bedrock to 
## Achieve the best price performance
SageMaker
- Instance-based
BedRock
- Token-based
## Accelerate AI Development
## Develop8ing proprietary
SageMaker
- You should have access to labeled data, e.g., huggingface, universities. 
- Select the fine-tuning techniques and algorithms, each has own use case, advantage and disadvantage
- SageMaker Clarify and SageMaker Model Monitor for Evaluation
BedRock
- Use proprietary data for RAG
- BedRock Guardrails
- Start small, expand and use supervisor agent
## Make generative AI work with your data
- RAG : Retrieve from your data source, augmented to LLM to generate the answer
- Fine-tuning : Historical labeled small number of data, data is not changing
- Continued pre-training : data is changing, unlabeled generalized data
## Solution diagram 
## Controlling Cost with Amazon BedRock
- Tracking / Controlloing Costs
Tag Bedrock Resources, AWS Budgets, Multiple accounts, CloudWatch
- Cross-Region inference & TPM
Provisioned Throughput, RPM/TPM Limit
Limitation Nova Pro ~ 3k API
Cross-Region : Use BedRock service in your own region and use model from another Region
Provisioned Throughput : if and only if your workload is consistent and high volume (discount cost)
- Token Tapering **
System Prompts, Token Parameters, Conversation History, Retreival Length
- Model Tapering
Model as a Judges, Personas to switch
