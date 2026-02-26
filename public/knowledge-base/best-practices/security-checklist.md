---
title: "Security Checklist"
title: "Security Checklist"
tags: [best-practices, security, aws, checklist]
created: 2025-12-18
updated: 2025-12-18
---
title: "Security Checklist"

# AWS Security Checklist

Essential security controls for AWS environments.

## Identity & Access Management

- [ ] Enable MFA on all IAM users (especially root)
- [ ] Use IAM roles instead of long-term credentials
- [ ] Implement least privilege principle
- [ ] Review IAM policies quarterly
- [ ] Delete unused IAM users and roles
- [ ] Rotate access keys every 90 days
- [ ] Use AWS Organizations with SCPs

## Logging & Monitoring

- [ ] Enable CloudTrail in all regions
- [ ] Enable CloudTrail log file validation
- [ ] Store CloudTrail logs in separate account
- [ ] Enable VPC Flow Logs
- [ ] Enable S3 access logging
- [ ] Enable RDS audit logging
- [ ] Set up CloudWatch alarms for security events

## Threat Detection

- [ ] Enable GuardDuty in all regions
- [ ] Enable Security Hub
- [ ] Enable AWS Config rules
- [ ] Configure SNS alerts for findings
- [ ] Review findings weekly

## Network Security

- [ ] Use private subnets for databases/backend
- [ ] Implement security groups with least privilege
- [ ] Use NACLs for additional layer
- [ ] Enable VPC endpoints for AWS services
- [ ] Disable public access to S3 buckets (unless required)
- [ ] Use WAF for public-facing applications

## Data Protection

- [ ] Enable encryption at rest (S3, RDS, EBS)
- [ ] Enable encryption in transit (TLS/SSL)
- [ ] Use KMS for key management
- [ ] Enable S3 versioning for critical buckets
- [ ] Configure S3 Object Lock for compliance
- [ ] Use Secrets Manager for credentials

## Compute Security

- [ ] Use latest AMIs with security patches
- [ ] Enable IMDSv2 for EC2 instances
- [ ] Scan container images for vulnerabilities
- [ ] Use Fargate for serverless containers
- [ ] Implement security groups per service

## Incident Response Preparation

- [ ] Document incident response procedures
- [ ] Set up incident notification channels
- [ ] Create runbooks for common scenarios
- [ ] Test backup restoration quarterly
- [ ] Conduct tabletop exercises annually

## Compliance

- [ ] Enable AWS Artifact for compliance reports
- [ ] Configure AWS Config for compliance rules
- [ ] Document data classification
- [ ] Implement data retention policies
- [ ] Review third-party access quarterly

## Quick Audit Commands

```bash
# Check for users without MFA
aws iam generate-credential-report
aws iam get-credential-report --query 'Content' --output text | base64 -d | grep -v "true"

# Check for public S3 buckets
aws s3api list-buckets --query 'Buckets[].Name' --output text | \
  xargs -I {} aws s3api get-bucket-acl --bucket {} 2>/dev/null

# Check for unused access keys
aws iam list-users --query 'Users[].UserName' --output text | \
  xargs -I {} aws iam list-access-keys --user-name {}

# Check CloudTrail status
aws cloudtrail describe-trails --query 'trailList[].{Name:Name,IsMultiRegion:IsMultiRegionTrail,IsLogging:IsLogging}'

# Check GuardDuty status
aws guardduty list-detectors
```

## Related

- [Security Incident Investigation](/knowledge-base/aws/security-incident-investigation.md)
- [Incident Response](/best-practices/incident-response.md)
