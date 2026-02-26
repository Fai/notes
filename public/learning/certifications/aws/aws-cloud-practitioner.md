# AWS Certified Cloud Practitioner (CLF-C02) Study Guide

Entry-level certification covering AWS Cloud fundamentals.

## Exam Overview

| Detail | Value |
|--------|-------|
| Duration | 90 minutes |
| Questions | 65 |
| Passing Score | 700/1000 |
| Cost | $100 |
| Prerequisites | None |

## Exam Domains

| Domain | Weight | Focus |
|--------|--------|-------|
| 1. Cloud Concepts | 24% | Cloud value proposition, AWS Well-Architected |
| 2. Security and Compliance | 30% | Shared responsibility, IAM, compliance |
| 3. Cloud Technology and Services | 34% | Compute, storage, networking, databases |
| 4. Billing, Pricing, and Support | 12% | Pricing models, support plans, cost tools |

---

## Domain 1: Cloud Concepts (24%)

### Benefits of AWS Cloud
- **Cost savings** - Pay-as-you-go, no upfront costs
- **Agility** - Deploy globally in minutes
- **Elasticity** - Scale up/down automatically
- **Reliability** - Multiple Availability Zones
- **Security** - Built-in compliance and encryption

### AWS Well-Architected Framework (6 Pillars)
| Pillar | Focus |
|--------|-------|
| Operational Excellence | Automate, monitor, improve |
| Security | Protect data and systems |
| Reliability | Recover from failures |
| Performance Efficiency | Use resources efficiently |
| Cost Optimization | Avoid unnecessary costs |
| Sustainability | Minimize environmental impact |

### Cloud Deployment Models
- **Public Cloud** - AWS, fully managed
- **Private Cloud** - On-premises, dedicated
- **Hybrid Cloud** - Mix of public and private

---

## Domain 2: Security and Compliance (30%)

### Shared Responsibility Model

| AWS Responsibility | Customer Responsibility |
|-------------------|------------------------|
| Physical security | Data encryption |
| Network infrastructure | IAM users and permissions |
| Hypervisor | Security groups |
| Managed services | Application security |

### IAM (Identity and Access Management)
- **Users** - Individual accounts
- **Groups** - Collection of users
- **Roles** - Temporary permissions (for services)
- **Policies** - JSON permission documents

### Key Security Services
| Service | Purpose |
|---------|---------|
| IAM | Access management |
| AWS Organizations | Multi-account management |
| AWS Shield | DDoS protection |
| AWS WAF | Web application firewall |
| Amazon Inspector | Vulnerability scanning |
| AWS CloudTrail | API activity logging |
| Amazon GuardDuty | Threat detection |

### Compliance
- **AWS Artifact** - Access compliance reports
- **AWS Config** - Track resource configurations
- Common certifications: SOC, PCI DSS, HIPAA, ISO

---

## Domain 3: Cloud Technology and Services (34%)

### Compute Services
| Service | Use Case |
|---------|----------|
| EC2 | Virtual servers |
| Lambda | Serverless functions |
| ECS/EKS | Container orchestration |
| Elastic Beanstalk | PaaS deployment |
| Lightsail | Simple VPS |

### Storage Services
| Service | Type | Use Case |
|---------|------|----------|
| S3 | Object | Files, backups, static websites |
| EBS | Block | EC2 disk volumes |
| EFS | File | Shared file system |
| S3 Glacier | Archive | Long-term backup |

### Database Services
| Service | Type |
|---------|------|
| RDS | Relational (MySQL, PostgreSQL, etc.) |
| DynamoDB | NoSQL key-value |
| Aurora | High-performance relational |
| ElastiCache | In-memory caching |
| Redshift | Data warehouse |

### Networking
| Service | Purpose |
|---------|---------|
| VPC | Virtual private network |
| CloudFront | CDN |
| Route 53 | DNS |
| API Gateway | API management |
| Direct Connect | Dedicated connection |

### Other Key Services
| Service | Purpose |
|---------|---------|
| CloudWatch | Monitoring and logs |
| SNS | Push notifications |
| SQS | Message queuing |
| CloudFormation | Infrastructure as Code |

---

## Domain 4: Billing and Pricing (12%)

### Pricing Models
| Model | Description |
|-------|-------------|
| On-Demand | Pay per hour/second, no commitment |
| Reserved | 1-3 year commitment, up to 72% savings |
| Spot | Bid on unused capacity, up to 90% savings |
| Savings Plans | Flexible commitment-based pricing |

### Cost Management Tools
| Tool | Purpose |
|------|---------|
| AWS Cost Explorer | Visualize spending |
| AWS Budgets | Set spending alerts |
| AWS Pricing Calculator | Estimate costs |
| Cost and Usage Report | Detailed billing data |

### Support Plans
| Plan | Response Time | Features |
|------|---------------|----------|
| Basic | None | Documentation, forums |
| Developer | 12-24 hours | Email support |
| Business | 1-4 hours | 24/7 phone, chat |
| Enterprise | 15 min (critical) | TAM, concierge |

### Free Tier
- **Always Free** - Lambda 1M requests, DynamoDB 25GB
- **12 Months Free** - EC2 750 hours, S3 5GB, RDS 750 hours
- **Trials** - Service-specific free trials

---

## Study Resources

### Free Resources
- [AWS Cloud Practitioner Essentials](https://explore.skillbuilder.aws/learn/course/external/view/elearning/134/aws-cloud-practitioner-essentials) (Skill Builder)
- [AWS Whitepapers](https://aws.amazon.com/whitepapers/)
- [AWS FAQs](https://aws.amazon.com/faqs/)

### Practice
- AWS Skill Builder practice exam
- Tutorials Dojo practice tests

### Study Time
- **Recommended:** 2-4 weeks
- **Daily:** 1-2 hours
- **Total:** 20-40 hours
