---
title: "AWS Certified Security Specialty Study Guide"
tags: [learning, certification, aws, security]
created: 2025-12-18
---

# AWS Certified Security Specialty (SCS-C02) Study Guide

Comprehensive study guide for the AWS Security Specialty certification.

## Exam Overview

| Detail | Value |
|--------|-------|
| Duration | 170 minutes |
| Questions | 65 (50 scored, 15 unscored) |
| Passing Score | 750/1000 |
| Cost | $300 |
| Format | Multiple choice/multiple response |

## Exam Domains

| Domain | Weight | Key Topics |
|--------|--------|------------|
| 1. Threat Detection & Incident Response | 14% | IR plans, threat detection, compromise response |
| 2. Security Logging & Monitoring | 18% | CloudTrail, CloudWatch, log analysis |
| 3. Infrastructure Security | 20% | VPC, WAF, Shield, network controls |
| 4. Identity & Access Management | 16% | IAM, STS, Cognito, Directory Services |
| 5. Data Protection | 18% | KMS, encryption, Secrets Manager |
| 6. Management & Security Governance | 14% | Config, Organizations, compliance |

## Study Timeline Options

Choose based on your availability:

| Plan | Duration | Hours/Week | Total Hours |
|------|----------|------------|-------------|
| Standard | 12-13 weeks | 15 (weekdays only) | ~195 |
| Moderate | 8-9 weeks | 19 (weekdays + light weekends) | ~152 |
| Intensive | 7-8 weeks | 25-27 (weekdays + heavy weekends) | ~200 |

## Week-by-Week Study Plan

### Weeks 1-2: IAM & Identity Foundation

**Topics:**
- IAM users, groups, roles, policies
- Policy evaluation logic
- Permission boundaries
- STS and assume role
- IAM Access Analyzer
- AWS Organizations & SCPs
- IAM Identity Center (SSO)
- Cognito user pools & identity pools
- Directory Service

**Hands-on Labs:**
- Create users, groups, policies
- Configure cross-account access with STS
- Set up permission boundaries
- Create Organizations with OUs and SCPs
- Configure Cognito user pool

**Practice Questions:** 50 questions on IAM & Organizations

---

### Weeks 3-4: Security Logging & Monitoring

**Topics:**
- CloudTrail configuration and analysis
- CloudWatch Logs, metrics, alarms
- VPC Flow Logs
- AWS Config rules
- Amazon Detective
- Security Hub
- Log aggregation patterns

**Hands-on Labs:**
- Enable CloudTrail with S3 and CloudWatch integration
- Create CloudWatch alarms for security events
- Configure VPC Flow Logs
- Set up AWS Config rules
- Enable Security Hub

**Practice Questions:** 50 questions on logging & monitoring

---

### Weeks 5-6: Infrastructure Security

**Topics:**
- VPC security (NACLs, security groups)
- AWS WAF rules and web ACLs
- AWS Shield Standard vs Advanced
- AWS Firewall Manager
- Network Firewall
- Route 53 DNS security
- CloudFront security
- API Gateway security

**Hands-on Labs:**
- Configure WAF with custom rules
- Set up Shield Advanced
- Create Network Firewall policies
- Secure API Gateway endpoints

**Practice Questions:** 50 questions on infrastructure security

---

### Weeks 7-8: Data Protection

**Topics:**
- KMS key types and key policies
- Envelope encryption
- S3 encryption options
- EBS/RDS encryption
- Secrets Manager vs Parameter Store
- ACM certificate management
- Macie for data discovery

**Hands-on Labs:**
- Create and manage KMS keys
- Implement S3 bucket encryption
- Store and rotate secrets in Secrets Manager
- Configure ACM certificates

**Practice Questions:** 50 questions on data protection

---

### Weeks 9-10: Threat Detection & Incident Response

**Topics:**
- GuardDuty findings and response
- Amazon Inspector
- Incident response procedures
- Forensics and evidence collection
- Automated remediation with Lambda
- EventBridge for security automation

**Hands-on Labs:**
- Enable and configure GuardDuty
- Set up automated remediation
- Create incident response runbooks
- Configure Inspector assessments

**Practice Questions:** 50 questions on threat detection & IR

---

### Weeks 11-12: Governance & Review

**Topics:**
- AWS Organizations best practices
- Service Control Policies (SCPs)
- AWS Control Tower
- Compliance frameworks
- AWS Artifact

**Final Preparation:**
- Full-length practice exams (3-4)
- Review weak areas
- Re-do failed practice questions
- Read AWS security whitepapers

---

## Key AWS Services to Master

### Identity & Access
- IAM (users, roles, policies, permission boundaries)
- STS (AssumeRole, GetFederationToken)
- Cognito (user pools, identity pools)
- IAM Identity Center
- Directory Service

### Detection & Monitoring
- CloudTrail
- CloudWatch (Logs, Metrics, Alarms)
- GuardDuty
- Security Hub
- Amazon Detective
- AWS Config

### Infrastructure Protection
- VPC (security groups, NACLs)
- WAF
- Shield
- Network Firewall
- Firewall Manager

### Data Protection
- KMS
- Secrets Manager
- Parameter Store
- ACM
- Macie

### Governance
- Organizations
- Control Tower
- AWS Config
- Service Catalog

---

## Recommended Resources

### AWS Skill Builder (Subscription)
- Security Specialty Exam Prep Course
- Security Learning Plan
- Hands-on labs

### Whitepapers
- AWS Security Best Practices
- AWS Well-Architected Security Pillar
- AWS KMS Best Practices

### Practice Exams
- AWS Skill Builder official practice exam
- Tutorials Dojo practice tests
- Whizlabs practice tests

---

## Exam Day Tips

1. **Time Management:** ~2.5 minutes per question
2. **Flag and Review:** Mark uncertain questions, review at end
3. **Eliminate Wrong Answers:** Narrow down to 2 options
4. **Read Carefully:** Look for keywords like "MOST secure", "LEAST operational overhead"
5. **AWS Best Practices:** When in doubt, choose the AWS-native solution
