---
title: Bedrock Cost Control & Budget Management
tags: [aws, bedrock, cost-management, budget, api-gateway, lambda]
created: 2025-12-18
---

# Bedrock Cost Control & Budget Management

Per-user token limits, budget alerts, and throttling for Amazon Bedrock.

## Multi-Layer Cost Control Architecture

```
Layer 1: AWS Budgets (Account-level)
    ↓
Layer 2: API Gateway Usage Plans (Per API Key)
    ↓
Layer 3: Application-Level Token Tracking (DynamoDB + Lambda)
    ↓
Layer 4: Cost Allocation Tags
```

## 1. AWS Budgets (Account-Level)

### Create Budget with Alerts

```bash
aws budgets create-budget \
  --account-id <ACCOUNT_ID> \
  --budget file://budget.json \
  --notifications-with-subscribers file://notifications.json
```

**budget.json:**
```json
{
  "BudgetName": "BedrockMonthlyBudget",
  "BudgetLimit": { "Amount": "1000", "Unit": "USD" },
  "TimeUnit": "MONTHLY",
  "BudgetType": "COST",
  "CostFilters": { "Service": ["Amazon Bedrock"] }
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
      { "SubscriptionType": "EMAIL", "Address": "<email>" },
      { "SubscriptionType": "SNS", "Address": "<sns-arn>" }
    ]
  }
]
```

## 2. API Gateway Usage Plans (Per API Key)

### Setup Usage Plan

```bash
# Create usage plan with throttling and quota
aws apigateway create-usage-plan \
  --name "BedrockBasicPlan" \
  --throttle burstLimit=10,rateLimit=5 \
  --quota limit=1000,period=DAY

# Create API key
aws apigateway create-api-key \
  --name "user-<name>" \
  --enabled

# Associate key with plan
aws apigateway create-usage-plan-key \
  --usage-plan-id <plan-id> \
  --key-id <key-id> \
  --key-type API_KEY
```

### Tier Examples

| Tier | Requests/Day | Rate/Second | ~Monthly Cost |
|------|-------------|-------------|---------------|
| Basic | 1,000 | 5 | $50 |
| Professional | 10,000 | 20 | $500 |
| Enterprise | 100,000 | 100 | $5,000 |

## 3. Application-Level Token Tracking

### DynamoDB Table

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

### Lambda Token Checker (Python)

```python
import boto3
import json
from datetime import datetime

dynamodb = boto3.resource('dynamodb')
bedrock = boto3.client('bedrock-runtime')
table = dynamodb.Table('user_token_usage')

TOKEN_LIMITS = {
    'basic': 100000,
    'professional': 1000000,
    'enterprise': 10000000
}

def lambda_handler(event, context):
    user_id = event['user_id']
    user_tier = event.get('tier', 'basic')
    prompt = event['prompt']
    current_month = datetime.now().strftime('%Y-%m')
    
    # Check current usage
    response = table.get_item(Key={'user_id': user_id, 'month': current_month})
    current_tokens = response.get('Item', {}).get('tokens', 0)
    token_limit = TOKEN_LIMITS[user_tier]
    
    if current_tokens >= token_limit:
        return {'statusCode': 429, 'body': json.dumps({'error': 'Token limit exceeded'})}
    
    # Call Bedrock
    bedrock_response = bedrock.invoke_model(
        modelId='anthropic.claude-3-sonnet-20240229-v1:0',
        body=json.dumps({
            'anthropic_version': 'bedrock-2023-05-31',
            'max_tokens': 1024,
            'messages': [{'role': 'user', 'content': prompt}]
        })
    )
    
    response_body = json.loads(bedrock_response['body'].read())
    total_tokens = response_body['usage']['input_tokens'] + response_body['usage']['output_tokens']
    
    # Update usage
    table.update_item(
        Key={'user_id': user_id, 'month': current_month},
        UpdateExpression='ADD tokens :tokens',
        ExpressionAttributeValues={':tokens': total_tokens}
    )
    
    return {'statusCode': 200, 'body': json.dumps({'response': response_body['content'][0]['text']})}
```

## 4. CloudWatch Monitoring

### Custom Metrics

```python
cloudwatch = boto3.client('cloudwatch')

def publish_token_usage(user_id, tokens, cost):
    cloudwatch.put_metric_data(
        Namespace='Bedrock/Usage',
        MetricData=[
            {'MetricName': 'TokensUsed', 'Dimensions': [{'Name': 'UserId', 'Value': user_id}], 'Value': tokens, 'Unit': 'Count'},
            {'MetricName': 'Cost', 'Dimensions': [{'Name': 'UserId', 'Value': user_id}], 'Value': cost, 'Unit': 'None'}
        ]
    )
```

### CloudWatch Alarm

```bash
aws cloudwatch put-metric-alarm \
  --alarm-name "user-token-limit-80pct" \
  --metric-name TokensUsed \
  --namespace Bedrock/Usage \
  --statistic Sum \
  --period 86400 \
  --threshold 80000 \
  --comparison-operator GreaterThanThreshold \
  --dimensions Name=UserId,Value=<user-id> \
  --alarm-actions <sns-arn>
```

## Quick Reference: Bedrock Pricing (Claude 3)

| Model | Input (per 1K tokens) | Output (per 1K tokens) |
|-------|----------------------|------------------------|
| Claude 3 Haiku | $0.00025 | $0.00125 |
| Claude 3 Sonnet | $0.003 | $0.015 |
| Claude 3 Opus | $0.015 | $0.075 |

## Related Articles

- [[bedrock-knowledge-bases]] - RAG with Bedrock
- [[ai-ml-governance]] - Enterprise AI governance
- [[cost-optimization]] - General AWS cost optimization
