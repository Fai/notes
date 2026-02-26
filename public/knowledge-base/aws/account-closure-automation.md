---
title: AWS Account Closure Automation
tags: [aws, organizations, account-management, automation, batch-operations]
created: 2025-12-18
---

# AWS Account Closure Automation

Guide for automating AWS account closure in batch operations via AWS Organizations.

## Critical Limitations

### Account Closure Quota

**Most Important**: You can close only **10% of member accounts** (between 10-1000) within a **rolling 30-day period**.

| Total Accounts | Max Closures/30 Days |
|---------------|---------------------|
| 100 | 10 |
| 500 | 50 |
| 5,000 | 500 |
| 10,000+ | 1,000 |

- Quota is **NOT adjustable**
- 30-day period is **rolling**, not calendar-month
- Exceeding quota produces `CLOSE_ACCOUNT_QUOTA_EXCEEDED` error

### Account Closure Timeline

```
Day 0:     Closure initiated → Status: PENDING_CLOSURE
Day 0-90:  Account suspended (grace period) → Status: SUSPENDED
           Can be reinstated by contacting AWS Support
Day 90+:   Permanently closed → Status: CLOSED
           Cannot be recovered
```

### Important Notes

- ❌ Cannot close management account via API
- ⚠️ Closing account does NOT stop all billing
- ⚠️ AWS Marketplace subscriptions continue
- ⚠️ Reserved Instances/Savings Plans commitments remain

## Prerequisites

- AWS Organizations with all features enabled
- Permission: `organizations:CloseAccount`
- Must call from management account
- Target must be member account

## AWS CLI

```bash
# Close single account
aws organizations close-account --account-id <ACCOUNT_ID>

# Check account status
aws organizations describe-account --account-id <ACCOUNT_ID>
```

## Python (Boto3) Batch Script

```python
import boto3
import time
from botocore.exceptions import ClientError

def close_account(account_id):
    client = boto3.client('organizations')
    try:
        client.close_account(AccountId=account_id)
        print(f"Initiated closure for: {account_id}")
        return True
    except ClientError as e:
        if e.response['Error']['Code'] == 'ConstraintViolationException':
            print("Quota exceeded!")
            return False
        raise

def get_account_status(account_id):
    client = boto3.client('organizations')
    response = client.describe_account(AccountId=account_id)
    return response['Account']['Status']

def close_accounts_batch(account_ids, dry_run=True):
    results = {'successful': [], 'failed': []}
    
    for account_id in account_ids:
        status = get_account_status(account_id)
        if status in ['SUSPENDED', 'PENDING_CLOSURE']:
            print(f"{account_id} already closing, skipping")
            continue
        
        if dry_run:
            print(f"[DRY RUN] Would close: {account_id}")
        else:
            if close_account(account_id):
                results['successful'].append(account_id)
                time.sleep(1)  # Rate limiting
            else:
                results['failed'].append(account_id)
                break  # Stop on quota exceeded
    
    return results

# Usage
accounts = ["123456789012", "234567890123"]
close_accounts_batch(accounts, dry_run=True)
```

## Pre-Closure Resource Cleanup

### Using AWS Nuke (Recommended)

```bash
# Install from: https://github.com/rebuy-de/aws-nuke

# Create config
cat > nuke-config.yml <<EOF
regions:
  - global
  - us-east-1
account-blocklist:
  - "999999999999"  # Production (safety)
accounts:
  "<ACCOUNT_ID>":
    filters:
      IAMRole:
        - type: "regex"
          value: "^OrganizationAccountAccessRole$"
EOF

# Dry run
aws-nuke -c nuke-config.yml --profile target-account

# Execute (DANGEROUS!)
aws-nuke -c nuke-config.yml --profile target-account --no-dry-run
```

### Using Cloud-Nuke (Alternative)

```bash
brew install cloud-nuke

# List resources
cloud-nuke aws --list-resource-types

# Delete all resources older than 2 days
cloud-nuke aws --older-than 2d
```

## Account States

| State | Description |
|-------|-------------|
| `ACTIVE` | Account operational |
| `PENDING_CLOSURE` | Closure in progress |
| `SUSPENDED` | Closed, 90-day grace period |
| `CLOSED` | Permanently closed |

## Best Practices

1. **Always dry-run first** - Test scripts before actual execution
2. **Clean up resources before closing** - Avoid unexpected charges
3. **Track quota usage** - Plan batch closures across multiple months
4. **Document closed accounts** - Keep records for compliance
5. **Notify stakeholders** - Ensure no active workloads remain

## Related Articles

- [[quicksight-administration]] - QuickSight batch unsubscribe
- [[security-incident-investigation]] - Security cleanup procedures
- [[cost-optimization]] - Cost management strategies
