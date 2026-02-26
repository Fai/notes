---
title: "Bedrock Cost Control Quick Reference"
tags: [research, aws, bedrock, cost]
created: 2025-12-18
---

# Bedrock Cost Control - Quick Reference Guide

## Disable API Key When User Reaches Limit

### Option 1: API Gateway Quota (Recommended - No Code)

**Automatically blocks requests at limit, auto-resets monthly**

```bash
aws apigateway create-usage-plan \
  --name "user-plan" \
  --quota limit=10000,period=MONTH \
  --throttle rateLimit=10,burstLimit=20
```

**Documentation:**
- https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-api-usage-plans.html

---

### Option 2: Programmatically Disable API Key

```bash
# Disable API key
aws apigateway update-api-key \
  --api-key YOUR_KEY_ID \
  --patch-operations op=replace,path=/enabled,value=false

# Enable API key
aws apigateway update-api-key \
  --api-key YOUR_KEY_ID \
  --patch-operations op=replace,path=/enabled,value=true
```

**Documentation:**
- https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateApiKey.html

---

### Option 3: Lambda Function (Automatic Disable)

```python
import boto3

apigateway = boto3.client('apigateway')

def disable_key(api_key_id):
    apigateway.update_api_key(
        apiKey=api_key_id,
        patchOperations=[{
            'op': 'replace',
            'path': '/enabled',
            'value': 'false'
        }]
    )
```

---

## AWS Budgets (Cost Alerts)

### Create Budget with Alerts

```bash
aws budgets create-budget \
  --account-id 123456789012 \
  --budget file://budget.json \
  --notifications-with-subscribers file://notifications.json
```

**budget.json:**
```json
{
  "BudgetName": "BedrockMonthlyBudget",
  "BudgetLimit": {
    "Amount": "1000",
    "Unit": "USD"
  },
  "TimeUnit": "MONTHLY",
  "BudgetType": "COST",
  "CostFilters": {
    "Service": ["Amazon Bedrock"]
  }
}
```

**notifications.json:**
```json
[
  {
    "Notification": {
      "NotificationType": "ACTUAL",
      "ComparisonOperator": "GREATER_THAN",
      "Threshold": 80,
      "ThresholdType": "PERCENTAGE"
    },
    "Subscribers": [
      {
        "SubscriptionType": "EMAIL",
        "Address": "admin@company.com"
      }
    ]
  }
]
```

**Documentation:**
- https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html
- https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-create.html

---

## Token Tracking with DynamoDB

### Create Table

```bash
aws dynamodb create-table \
  --table-name user_token_usage \
  --attribute-definitions \
    AttributeName=user_id,AttributeType=S \
    AttributeName=month,AttributeType=S \
  --key-schema \
    AttributeName=user_id,KeyType=HASH \
    AttributeName=month,KeyType=RANGE \
  --billing-mode PAY_PER_REQUEST
```

### Lambda Function: Check and Disable

```python
import boto3
from datetime import datetime

dynamodb = boto3.resource('dynamodb')
apigateway = boto3.client('apigateway')
table = dynamodb.Table('user_token_usage')

def check_limit(user_id, api_key_id, tokens_used, token_limit):
    current_month = datetime.now().strftime('%Y-%m')
    
    # Get current usage
    response = table.get_item(
        Key={'user_id': user_id, 'month': current_month}
    )
    total_tokens = response.get('Item', {}).get('tokens', 0) + tokens_used
    
    # Update usage
    table.update_item(
        Key={'user_id': user_id, 'month': current_month},
        UpdateExpression='SET tokens = :tokens',
        ExpressionAttributeValues={':tokens': total_tokens}
    )
    
    # Check if limit exceeded
    if total_tokens >= token_limit:
        # Disable API key
        apigateway.update_api_key(
            apiKey=api_key_id,
            patchOperations=[{
                'op': 'replace',
                'path': '/enabled',
                'value': 'false'
            }]
        )
        return {'status': 'disabled', 'tokens': total_tokens}
    
    return {'status': 'active', 'tokens': total_tokens}
```

---

## CloudWatch Alarms

### Create Alarm for Token Usage

```bash
aws cloudwatch put-metric-alarm \
  --alarm-name "user-token-limit-80pct" \
  --metric-name TokensUsed \
  --namespace Bedrock/Usage \
  --statistic Sum \
  --period 86400 \
  --evaluation-periods 1 \
  --threshold 80000 \
  --comparison-operator GreaterThanThreshold \
  --dimensions Name=UserId,Value=john-doe \
  --alarm-actions arn:aws:sns:ap-southeast-7:123456789012:alerts
```

**Documentation:**
- https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html

---

## AWS Documentation Links

### Core Services
1. **API Gateway Usage Plans**
   - https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-api-usage-plans.html
   - https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-request-throttling.html

2. **AWS Budgets**
   - https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html
   - https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-controls.html

3. **Amazon Bedrock**
   - https://aws.amazon.com/bedrock/pricing/
   - https://docs.aws.amazon.com/bedrock/latest/userguide/prov-throughput.html
   - https://docs.aws.amazon.com/bedrock/latest/userguide/quotas.html

4. **Cost Management**
   - https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html
   - https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html

5. **SageMaker Unified Studio**
   - https://docs.aws.amazon.com/sagemaker-unified-studio/latest/userguide/what-is-sagemaker-unified-studio.html
   - https://docs.aws.amazon.com/sagemaker-unified-studio/latest/userguide/bedrock.html

### API References
- **API Gateway**: https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateApiKey.html
- **Budgets**: https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_budgets_CreateBudget.html
- **Bedrock**: https://docs.aws.amazon.com/bedrock/latest/APIReference/API_runtime_InvokeModel.html

### Best Practices
- **Cost Optimization**: https://docs.aws.amazon.com/wellarchitected/latest/cost-optimization-pillar/welcome.html
- **Generative AI Lens**: https://docs.aws.amazon.com/wellarchitected/latest/generative-ai-lens/generative-ai-lens.html

---

## Quick Summary

**Question:** Can we disable API keys when users reach their limit?

**Answer:** Yes! Three options:

1. ✅ **API Gateway Quota** (Easiest) - Automatic blocking, no code
2. ✅ **AWS CLI** - Manual disable/enable
3. ✅ **Lambda Function** - Automatic disable based on token usage

**Recommended:** Use API Gateway Usage Plans with quotas - simplest solution, automatically enforced.

**Setup Time:** 30 minutes - 1 hour

**Cost:** ~$3-5/month for API Gateway
