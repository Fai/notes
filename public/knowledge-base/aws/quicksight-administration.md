---
title: QuickSight Administration & Batch Operations
tags: [aws, quicksight, administration, batch-operations, organizations]
created: 2025-12-18
---

# QuickSight Administration & Batch Operations

Guide for managing QuickSight subscriptions across multiple AWS accounts.

## ⚠️ Critical Warnings

- **Irreversible**: Deleting QuickSight subscription permanently deletes ALL data
- **Global deletion**: Runs from any region, deletes data in ALL regions
- **Immediate impact**: Embedded dashboards stop working instantly
- **No grace period**: Unlike account closure (90 days), deletion is immediate

### What Gets Deleted

- All users and administrators
- All dashboards and analyses
- All datasets and data sources
- All embedded dashboards

### What Is NOT Deleted

- AWS account (remains active)
- Other AWS services
- Data stored outside QuickSight (S3, RDS, etc.)

## Prerequisites

### Required IAM Permissions

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": [
      "quicksight:DeleteAccountSubscription",
      "quicksight:UpdateAccountSettings",
      "quicksight:DescribeAccountSettings"
    ],
    "Resource": "*"
  }]
}
```

## Single Account Cancellation

### AWS CLI

```bash
# Check current settings
aws quicksight describe-account-settings \
  --aws-account-id <ACCOUNT_ID>

# Disable termination protection
aws quicksight update-account-settings \
  --aws-account-id <ACCOUNT_ID> \
  --default-namespace default \
  --termination-protection-enabled false

# Delete subscription
aws quicksight delete-account-subscription \
  --aws-account-id <ACCOUNT_ID>
```

### Python (Boto3)

```python
import boto3
from botocore.exceptions import ClientError

def delete_quicksight_subscription(account_id, region='us-east-1'):
    client = boto3.client('quicksight', region_name=region)
    
    try:
        # Disable termination protection
        client.update_account_settings(
            AwsAccountId=account_id,
            DefaultNamespace='default',
            TerminationProtectionEnabled=False
        )
        
        # Delete subscription
        response = client.delete_account_subscription(AwsAccountId=account_id)
        return {'success': True, 'request_id': response['RequestId']}
    
    except ClientError as e:
        return {'success': False, 'error': e.response['Error']['Message']}
```

## Batch Multi-Account Cancellation

### Architecture

```
Management Account
    └─ Lambda/Script
         ↓ AssumeRole
    Member Account 1 → QuickSight deletion
         ↓ AssumeRole
    Member Account 2 → QuickSight deletion
```

### Step 1: Deploy IAM Role via StackSets

```yaml
# quicksight-deletion-role.yaml
AWSTemplateFormatVersion: '2010-09-09'
Parameters:
  ManagementAccountId:
    Type: String

Resources:
  QuickSightDeletionRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: QuickSightDeletionRole
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal:
              AWS: !Sub 'arn:aws:iam::${ManagementAccountId}:root'
            Action: sts:AssumeRole
            Condition:
              StringEquals:
                sts:ExternalId: 'quicksight-deletion'
      Policies:
        - PolicyName: QuickSightDeletionPolicy
          PolicyDocument:
            Version: '2012-10-17'
            Statement:
              - Effect: Allow
                Action:
                  - quicksight:DeleteAccountSubscription
                  - quicksight:UpdateAccountSettings
                  - quicksight:DescribeAccountSettings
                Resource: '*'
```

### Step 2: Batch Deletion Script

```python
import boto3
import time
from botocore.exceptions import ClientError

class QuickSightBatchDeleter:
    def __init__(self, role_name='QuickSightDeletionRole', external_id='quicksight-deletion'):
        self.role_name = role_name
        self.external_id = external_id
        self.sts = boto3.client('sts')
    
    def assume_role(self, account_id):
        role_arn = f'arn:aws:iam::{account_id}:role/{self.role_name}'
        response = self.sts.assume_role(
            RoleArn=role_arn,
            RoleSessionName='QuickSightDeletion',
            ExternalId=self.external_id
        )
        return response['Credentials']
    
    def delete_subscription(self, account_id, dry_run=True):
        if dry_run:
            print(f"[DRY RUN] Would delete QuickSight in {account_id}")
            return {'success': True, 'dry_run': True}
        
        creds = self.assume_role(account_id)
        client = boto3.client('quicksight',
            aws_access_key_id=creds['AccessKeyId'],
            aws_secret_access_key=creds['SecretAccessKey'],
            aws_session_token=creds['SessionToken']
        )
        
        # Disable protection and delete
        client.update_account_settings(
            AwsAccountId=account_id,
            DefaultNamespace='default',
            TerminationProtectionEnabled=False
        )
        client.delete_account_subscription(AwsAccountId=account_id)
        return {'success': True}
    
    def batch_delete(self, account_ids, dry_run=True):
        results = {'successful': [], 'failed': []}
        
        for account_id in account_ids:
            try:
                result = self.delete_subscription(account_id, dry_run)
                if result['success']:
                    results['successful'].append(account_id)
            except Exception as e:
                results['failed'].append({'account': account_id, 'error': str(e)})
            time.sleep(1)  # Rate limiting
        
        return results

# Usage
deleter = QuickSightBatchDeleter()
accounts = ["123456789012", "234567890123"]
results = deleter.batch_delete(accounts, dry_run=True)
```

## Pre-Deletion Checklist

- [ ] Export critical dashboards (PDF/screenshots)
- [ ] Document data sources and connection strings
- [ ] Notify stakeholders about embedded dashboard disruption
- [ ] Get written approval from management
- [ ] Test in non-production account first

## QuickSight Editions

| Edition | DeleteAccountSubscription API |
|---------|------------------------------|
| Standard | Limited/May have errors |
| Enterprise | Fully supported |

## Related Articles

- [[account-closure-automation]] - AWS account closure
- [[cost-optimization]] - Cost management strategies
