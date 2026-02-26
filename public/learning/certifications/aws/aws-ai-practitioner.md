---
title: "AWS Certified AI Practitioner (AIF-C01) Study Guide"
tags: [learning, certification, aws, ai]
created: 2025-12-18
---

# AWS Certified AI Practitioner (AIF-C01) Study Guide

Foundational certification covering AI/ML concepts and AWS AI services.

## Exam Overview

| Detail | Value |
|--------|-------|
| Duration | 90 minutes |
| Questions | 65 |
| Passing Score | 700/1000 |
| Cost | $100 |
| Prerequisites | None (Cloud Practitioner recommended) |

## Exam Domains

| Domain | Weight | Focus |
|--------|--------|-------|
| 1. Fundamentals of AI and ML | 20% | AI/ML concepts, terminology |
| 2. Fundamentals of Generative AI | 24% | LLMs, foundation models, prompting |
| 3. Applications of Foundation Models | 28% | Amazon Bedrock, use cases |
| 4. Guidelines for Responsible AI | 14% | Ethics, bias, transparency |
| 5. Security, Compliance, and Governance | 14% | Data privacy, model security |

---

## Domain 1: AI and ML Fundamentals (20%)

### AI vs ML vs Deep Learning
| Term | Definition |
|------|------------|
| AI | Machines performing human-like tasks |
| ML | Systems learning from data without explicit programming |
| Deep Learning | ML using neural networks with many layers |

### Types of Machine Learning
| Type | Description | Example |
|------|-------------|---------|
| Supervised | Labeled training data | Classification, regression |
| Unsupervised | No labels, find patterns | Clustering, anomaly detection |
| Reinforcement | Learn from rewards/penalties | Game playing, robotics |

### ML Pipeline
1. **Data Collection** - Gather training data
2. **Data Preparation** - Clean, transform, split
3. **Model Training** - Algorithm learns patterns
4. **Evaluation** - Test accuracy, precision, recall
5. **Deployment** - Put model into production
6. **Monitoring** - Track performance over time

### Key Metrics
| Metric | Use Case |
|--------|----------|
| Accuracy | Overall correctness |
| Precision | When false positives are costly |
| Recall | When false negatives are costly |
| F1 Score | Balance of precision and recall |

---

## Domain 2: Generative AI Fundamentals (24%)

### Foundation Models
- Large pre-trained models (billions of parameters)
- Can be fine-tuned for specific tasks
- Examples: Claude, Llama, Titan, Stable Diffusion

### Large Language Models (LLMs)
- Trained on massive text datasets
- Generate human-like text
- Capabilities: summarization, translation, Q&A, code generation

### Key Concepts
| Concept | Description |
|---------|-------------|
| Tokens | Units of text (words or subwords) |
| Context Window | Maximum input length |
| Temperature | Controls randomness (0=deterministic, 1=creative) |
| Top-p | Nucleus sampling for diversity |
| Inference | Using a trained model to generate outputs |

### Prompt Engineering
| Technique | Description |
|-----------|-------------|
| Zero-shot | No examples provided |
| Few-shot | Include examples in prompt |
| Chain-of-thought | Ask model to explain reasoning |
| System prompts | Set behavior and constraints |

### RAG (Retrieval Augmented Generation)
- Combines LLM with external knowledge base
- Reduces hallucinations
- Provides up-to-date information
- Components: Vector store, embeddings, retriever

---

## Domain 3: Applications of Foundation Models (28%)

### Amazon Bedrock
- Fully managed service for foundation models
- Access models from: Anthropic, AI21, Cohere, Meta, Stability AI, Amazon
- No infrastructure management

### Bedrock Features
| Feature | Purpose |
|---------|---------|
| Knowledge Bases | Managed RAG |
| Agents | Multi-step task automation |
| Guardrails | Content filtering, safety |
| Fine-tuning | Customize models |
| Prompt Management | Version and manage prompts |

### Other AWS AI Services
| Service | Use Case |
|---------|----------|
| Amazon Q | AI assistant for business |
| Amazon SageMaker | Build/train/deploy ML models |
| Amazon Rekognition | Image and video analysis |
| Amazon Comprehend | NLP, sentiment analysis |
| Amazon Transcribe | Speech to text |
| Amazon Polly | Text to speech |
| Amazon Translate | Language translation |
| Amazon Textract | Document text extraction |
| Amazon Lex | Conversational interfaces |
| Amazon Personalize | Recommendations |
| Amazon Forecast | Time series forecasting |

### Use Cases
| Use Case | Services |
|----------|----------|
| Chatbots | Bedrock, Lex |
| Content generation | Bedrock |
| Code assistance | Amazon Q Developer, Bedrock |
| Document processing | Textract, Comprehend |
| Image generation | Bedrock (Stable Diffusion, Titan) |
| Search enhancement | Bedrock Knowledge Bases |

---

## Domain 4: Responsible AI (14%)

### Key Principles
- **Fairness** - Avoid bias in training data and outputs
- **Transparency** - Explain how models make decisions
- **Privacy** - Protect personal data
- **Accountability** - Human oversight of AI systems
- **Safety** - Prevent harmful outputs

### Types of Bias
| Bias Type | Description |
|-----------|-------------|
| Training data bias | Unrepresentative data |
| Selection bias | Skewed sampling |
| Confirmation bias | Reinforcing existing beliefs |
| Algorithmic bias | Model amplifies biases |

### Mitigation Strategies
- Diverse training datasets
- Regular bias audits
- Human review of outputs
- Guardrails and content filters
- Model cards and documentation

### AWS Tools for Responsible AI
| Tool | Purpose |
|------|---------|
| SageMaker Clarify | Bias detection and explainability |
| Bedrock Guardrails | Content filtering |
| Model Cards | Document model behavior |

---

## Domain 5: Security and Governance (14%)

### Data Security
- Encryption at rest and in transit
- VPC endpoints for private access
- IAM policies for access control
- Data residency considerations

### Model Security
- Prompt injection prevention
- Output filtering
- Rate limiting
- Audit logging

### Compliance
- Model invocation logging (CloudTrail)
- Data retention policies
- Industry regulations (HIPAA, GDPR, etc.)

### AWS Security Services
| Service | Purpose |
|---------|---------|
| IAM | Access control |
| KMS | Encryption key management |
| CloudTrail | API logging |
| VPC | Network isolation |
| Bedrock Guardrails | Content safety |

---

## Study Resources

### Free Resources
- [AWS AI Practitioner Essentials](https://explore.skillbuilder.aws/) (Skill Builder)
- [Generative AI Learning Plan](https://explore.skillbuilder.aws/)
- [Amazon Bedrock Documentation](https://docs.aws.amazon.com/bedrock/)

### Hands-on Practice
- Amazon Bedrock console (playground)
- Build a simple chatbot
- Try different foundation models

### Study Time
- **Recommended:** 2-4 weeks
- **Daily:** 1-2 hours
- **Total:** 20-40 hours

### Prerequisites Knowledge
- Basic cloud concepts (Cloud Practitioner level)
- Understanding of AI/ML terminology
- Familiarity with AWS console
