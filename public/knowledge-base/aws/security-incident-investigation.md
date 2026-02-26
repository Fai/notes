---
tags: [aws, security, incident-response, cloudtrail, iam]
created: 2025-12-18
updated: 2025-12-18
---

# AWS Security Incident Investigation Guide

Systematic approach to investigating AWS security incidents.

## Initial Triage

### Key Questions
1. What was detected? (Suspicious roles, unauthorized access, unusual activity)
2. When was it detected? (Timeline is critical)
3. What resources are affected?
4. Is the threat still active?

### Immediate Actions
- [ ] Preserve CloudTrail logs
- [ ] Identify affected accounts/resources
- [ ] Assess blast radius
- [ ] Engage incident response team

## CloudTrail Investigation

### Essential Queries

#### Find IAM Role Creation
```bash
aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=EventName,AttributeValue=CreateRole \
  --start-time $(date -u -d '7 days ago' +%Y-%m-%dT%H:%M:%SZ) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%SZ)
```

#### Find Lambda Code Updates
```bash
aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=EventName,AttributeValue=UpdateFunctionCode20150331v2 \
  --start-time $(date -u -d '30 days ago' +%Y-%m-%dT%H:%M:%SZ)
```

#### Find User Creation
```bash
aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=EventName,AttributeValue=CreateUser \
  --start-time $(date -u -d '90 days ago' +%Y-%m-%dT%H:%M:%SZ)
```

### Key Fields to Extract
- `eventTime` - When it happened
- `userIdentity.userName` - Who did it
- `sourceIPAddress` - From where
- `userAgent` - Console, CLI, or SDK
- `requestParameters` - What was requested
- `responseElements` - What was created

## Common Attack Patterns

### 1. Compromised IAM User Credentials
**Indicators:**
- Unusual login times/locations
- API calls from unexpected IPs
- Creation of new IAM users/roles
- Access key usage from multiple IPs

**Investigation:**
```bash
# Check user's recent activity
aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=Username,AttributeValue=<username>
```

### 2. Lambda Function Compromise
**Indicators:**
- Unexpected code updates
- Lambda creating IAM resources
- Unusual invocation patterns
- Dependencies with known vulnerabilities

**Investigation:**
```bash
# Check Lambda modifications
aws lambda list-versions-by-function --function-name <function>

# Get function code
aws lambda get-function --function-name <function> --query 'Code.Location'
```

### 3. Rogue IAM User Creation
**Red Flags:**
- Generic names like "aws_consoler", "admin", "backup"
- Users in /service-role/ namespace
- Users with AdministratorAccess
- Users created outside business hours

**Investigation:**
```bash
# List all IAM users
aws iam list-users

# Check user creation date
aws iam get-user --user-name <username>

# List user's access keys
aws iam list-access-keys --user-name <username>
```

### 4. Federation Token Abuse
**Indicators:**
- GetFederationToken calls
- AssumeRole to unexpected roles
- Cross-account access attempts

## Evidence Collection

### What to Preserve
1. CloudTrail logs (export to S3)
2. Lambda function code (all versions)
3. IAM policies and trust relationships
4. VPC Flow Logs
5. CloudWatch Logs

### Export CloudTrail to S3
```bash
aws cloudtrail create-trail \
  --name incident-investigation \
  --s3-bucket-name <bucket> \
  --include-global-service-events
```

## Containment Actions

### Immediate (if active threat)
```bash
# Disable compromised user
aws iam update-login-profile --user-name <user> --password-reset-required

# Deactivate access keys
aws iam update-access-key --user-name <user> --access-key-id <key> --status Inactive

# Delete suspicious roles
aws iam delete-role --role-name <role>

# Disable Lambda
aws lambda put-function-concurrency --function-name <function> --reserved-concurrent-executions 0
```

### After Containment
- Rotate all credentials
- Review and tighten IAM policies
- Enable MFA on all users
- Review SCPs (Service Control Policies)

## Post-Incident

### Documentation
- Complete timeline of events
- Attack vector identified
- Affected resources
- Actions taken
- Lessons learned

### Prevention
- Enable GuardDuty
- Enable Security Hub
- Implement least privilege
- Regular access reviews
- Enable CloudTrail in all regions

## Related
- [Incident Response](/best-practices/incident-response.md)
- IAM Best Practices
