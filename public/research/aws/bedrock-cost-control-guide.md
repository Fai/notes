---
title: "Cost Control & Budget Management for Bedrock/SageMaker"
tags: [research, aws, bedrock, cost]
created: 2025-12-18
---

# Cost Control & Budget Management for Bedrock/SageMaker
## Per-User Token Limits, Budget Alerts, and Throttling

---

## Overview

Yes, you can implement comprehensive cost controls including:
- ✅ **Budget alerts** when reaching thresholds
- ✅ **Token/request limits** per API key
- ✅ **Automatic throttling** when limits reached
- ✅ **Per-user cost tracking**
- ✅ **Automated actions** when budget exceeded

---

## Solution Architecture

### Multi-Layer Cost Control

```
┌─────────────────────────────────────────────────────────┐
│  Layer 1: AWS Budgets (Account-level)                   │
│  - Set monthly budget                                    │
│  - Email/SNS alerts at 50%, 80%, 100%                   │
│  - Automated actions (optional)                          │
└─────────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│  Layer 2: API Gateway Usage Plans (Per API Key)         │
│  - Request throttling (requests/second)                  │
│  - Quota limits (requests/day or month)                  │
│  - Per-key tracking                                      │
└─────────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│  Layer 3: Application-Level Token Tracking              │
│  - DynamoDB: Track tokens per user                       │
│  - Lambda: Check limits before Bedrock call              │
│  - CloudWatch: Monitor and alert                         │
└─────────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│  Layer 4: Cost Allocation Tags                          │
│  - Tag resources by user/team/project                    │
│  - Cost Explorer: Detailed cost breakdown                │
│  - Chargeback reports                                    │
└─────────────────────────────────────────────────────────┘
```

---

## Implementation Guide

### 1. AWS Budgets (Account-Level Alerts)

#### Setup Budget with Alerts

**Step 1: Create Budget**
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
      "Threshold": 50,
      "ThresholdType": "PERCENTAGE"
    },
    "Subscribers": [
      {
        "SubscriptionType": "EMAIL",
        "Address": "team@example.com"
      },
      {
        "SubscriptionType": "SNS",
        "Address": "arn:aws:sns:ap-southeast-7:123456789012:budget-alerts"
      }
    ]
  },
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
        "Address": "admin@example.com"
      }
    ]
  },
  {
    "Notification": {
      "NotificationType": "ACTUAL",
      "ComparisonOperator": "GREATER_THAN",
      "Threshold": 100,
      "ThresholdType": "PERCENTAGE"
    },
    "Subscribers": [
      {
        "SubscriptionType": "EMAIL",
        "Address": "admin@example.com"
      }
    ]
  }
]
```

#### Budget Actions (Automated Response)

**Automatically disable API access when budget exceeded:**

```bash
aws budgets create-budget-action \
  --account-id 123456789012 \
  --budget-name BedrockMonthlyBudget \
  --notification-type ACTUAL \
  --action-type APPLY_IAM_POLICY \
  --action-threshold 100 \
  --definition file://budget-action.json
```

**budget-action.json:**
```json
{
  "IamActionDefinition": {
    "PolicyArn": "arn:aws:iam::123456789012:policy/DenyBedrockAccess"
  }
}
```

---

### 2. API Gateway Usage Plans (Per API Key Limits)

#### Architecture

```
User Request → API Gateway (with API Key)
                    ↓
            Check Usage Plan Limits
                    ↓
        [Within Limits?]
         ↙           ↘
       Yes            No
        ↓              ↓
    Forward to      Return 429
    Lambda/Bedrock  (Too Many Requests)
```

#### Setup Usage Plan

**Step 1: Create Usage Plan**
```bash
aws apigateway create-usage-plan \
  --name "BedrockBasicPlan" \
  --description "Basic tier: 1000 requests/day" \
  --throttle burstLimit=10,rateLimit=5 \
  --quota limit=1000,period=DAY
```

**Parameters:**
- `rateLimit`: 5 requests per second (steady state)
- `burstLimit`: 10 requests (burst capacity)
- `quota`: 1000 requests per day

**Step 2: Create API Key**
```bash
aws apigateway create-api-key \
  --name "user-john-doe" \
  --description "API key for John Doe" \
  --enabled
```

**Step 3: Associate API Key with Usage Plan**
```bash
aws apigateway create-usage-plan-key \
  --usage-plan-id abc123 \
  --key-id xyz789 \
  --key-type API_KEY
```

#### Multiple Tiers Example

**Tier 1: Basic (Free)**
- 1,000 requests/day
- 5 requests/second
- ~$50/month equivalent

**Tier 2: Professional**
- 10,000 requests/day
- 20 requests/second
- ~$500/month equivalent

**Tier 3: Enterprise**
- 100,000 requests/day
- 100 requests/second
- ~$5,000/month equivalent

---

### 3. Application-Level Token Tracking

#### Architecture

```
┌──────────────────────────────────────────────────────┐
│  DynamoDB Table: user_token_usage                     │
│  ┌────────────┬──────────┬──────────┬──────────────┐│
│  │ user_id    │ month    │ tokens   │ cost_usd     ││
│  ├────────────┼──────────┼──────────┼──────────────┤│
│  │ john-doe   │ 2025-12  │ 150000   │ 12.50        ││
│  │ jane-smith │ 2025-12  │ 80000    │ 6.40         ││
│  └────────────┴──────────┴──────────┴──────────────┘│
└──────────────────────────────────────────────────────┘
```

#### Lambda Function: Token Limit Checker

**lambda_function.py:**
```python
import boto3
import json
from datetime import datetime

dynamodb = boto3.resource('dynamodb')
bedrock = boto3.client('bedrock-runtime')
table = dynamodb.Table('user_token_usage')

# Token limits per user tier
TOKEN_LIMITS = {
    'basic': 100000,      # 100K tokens/month
    'professional': 1000000,  # 1M tokens/month
    'enterprise': 10000000    # 10M tokens/month
}

def lambda_handler(event, context):
    user_id = event['user_id']
    user_tier = event.get('tier', 'basic')
    prompt = event['prompt']
    
    # Get current month
    current_month = datetime.now().strftime('%Y-%m')
    
    # Check current usage
    response = table.get_item(
        Key={
            'user_id': user_id,
            'month': current_month
        }
    )
    
    current_tokens = response.get('Item', {}).get('tokens', 0)
    token_limit = TOKEN_LIMITS[user_tier]
    
    # Check if user exceeded limit
    if current_tokens >= token_limit:
        return {
            'statusCode': 429,
            'body': json.dumps({
                'error': 'Token limit exceeded',
                'current_usage': current_tokens,
                'limit': token_limit,
                'reset_date': f'{current_month}-01'
            })
        }
    
    # Estimate tokens for this request (rough estimate)
    estimated_tokens = len(prompt.split()) * 1.3
    
    # Check if this request would exceed limit
    if current_tokens + estimated_tokens > token_limit:
        return {
            'statusCode': 429,
            'body': json.dumps({
                'error': 'This request would exceed your token limit',
                'current_usage': current_tokens,
                'limit': token_limit,
                'remaining': token_limit - current_tokens
            })
        }
    
    # Call Bedrock
    try:
        bedrock_response = bedrock.invoke_model(
            modelId='anthropic.claude-3-sonnet-20240229-v1:0',
            body=json.dumps({
                'anthropic_version': 'bedrock-2023-05-31',
                'max_tokens': 1024,
                'messages': [{'role': 'user', 'content': prompt}]
            })
        )
        
        response_body = json.loads(bedrock_response['body'].read())
        
        # Get actual token usage from response
        input_tokens = response_body['usage']['input_tokens']
        output_tokens = response_body['usage']['output_tokens']
        total_tokens = input_tokens + output_tokens
        
        # Calculate cost (Claude 3 Sonnet pricing)
        input_cost = (input_tokens / 1000) * 0.003
        output_cost = (output_tokens / 1000) * 0.015
        total_cost = input_cost + output_cost
        
        # Update usage in DynamoDB
        table.update_item(
            Key={
                'user_id': user_id,
                'month': current_month
            },
            UpdateExpression='ADD tokens :tokens, cost_usd :cost',
            ExpressionAttributeValues={
                ':tokens': total_tokens,
                ':cost': round(total_cost, 4)
            }
        )
        
        return {
            'statusCode': 200,
            'body': json.dumps({
                'response': response_body['content'][0]['text'],
                'usage': {
                    'tokens_used': total_tokens,
                    'cost': round(total_cost, 4),
                    'remaining_tokens': token_limit - (current_tokens + total_tokens)
                }
            })
        }
        
    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)})
        }
```

#### DynamoDB Table Schema

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

---

### 4. CloudWatch Monitoring & Alerts

#### Custom Metrics

**Publish token usage to CloudWatch:**

```python
import boto3

cloudwatch = boto3.client('cloudwatch')

def publish_token_usage(user_id, tokens, cost):
    cloudwatch.put_metric_data(
        Namespace='Bedrock/Usage',
        MetricData=[
            {
                'MetricName': 'TokensUsed',
                'Dimensions': [
                    {'Name': 'UserId', 'Value': user_id}
                ],
                'Value': tokens,
                'Unit': 'Count'
            },
            {
                'MetricName': 'Cost',
                'Dimensions': [
                    {'Name': 'UserId', 'Value': user_id}
                ],
                'Value': cost,
                'Unit': 'None'
            }
        ]
    )
```

#### CloudWatch Alarms

**Alert when user reaches 80% of token limit:**

```bash
aws cloudwatch put-metric-alarm \
  --alarm-name "user-john-doe-token-limit-80pct" \
  --alarm-description "Alert when John Doe uses 80% of tokens" \
  --metric-name TokensUsed \
  --namespace Bedrock/Usage \
  --statistic Sum \
  --period 86400 \
  --evaluation-periods 1 \
  --threshold 80000 \
  --comparison-operator GreaterThanThreshold \
  --dimensions Name=UserId,Value=john-doe \
  --alarm-actions arn:aws:sns:ap-southeast-7:123456789012:token-alerts
```

---

### 5. Cost Allocation Tags

#### Tag Resources by User/Team

**Tag Bedrock invocations:**

```python
import boto3

bedrock = boto3.client('bedrock-runtime')

response = bedrock.invoke_model(
    modelId='anthropic.claude-3-sonnet-20240229-v1:0',
    body=json.dumps({...}),
    # Add cost allocation tags
    tags={
        'user_id': 'john-doe',
        'team': 'customer-support',
        'project': 'chatbot-v2',
        'cost_center': 'CC-1234'
    }
)
```

#### Cost Explorer Reports

**View costs by user:**
```bash
aws ce get-cost-and-usage \
  --time-period Start=2025-12-01,End=2025-12-31 \
  --granularity MONTHLY \
  --metrics BlendedCost \
  --group-by Type=TAG,Key=user_id
```

---

## Complete Implementation Example

### Scenario: Banking Chatbot with Per-User Limits

**Requirements:**
- 100 customer service agents
- Each agent: max 50,000 tokens/month (~$4/month)
- Total budget: $500/month
- Alert at 80% usage
- Block at 100% usage

**Implementation:**

```python
# config.py
USER_TIERS = {
    'agent': {
        'monthly_tokens': 50000,
        'monthly_cost': 4.00,
        'rate_limit': 5,  # requests/second
        'burst_limit': 10
    },
    'supervisor': {
        'monthly_tokens': 200000,
        'monthly_cost': 16.00,
        'rate_limit': 20,
        'burst_limit': 40
    }
}

ALERT_THRESHOLDS = [0.5, 0.8, 0.9, 1.0]  # 50%, 80%, 90%, 100%

# main.py
import boto3
from datetime import datetime
import json

class BedrockCostController:
    def __init__(self):
        self.dynamodb = boto3.resource('dynamodb')
        self.bedrock = boto3.client('bedrock-runtime')
        self.sns = boto3.client('sns')
        self.table = self.dynamodb.Table('user_token_usage')
        
    def check_and_invoke(self, user_id, tier, prompt):
        # Get current usage
        current_month = datetime.now().strftime('%Y-%m')
        usage = self.get_usage(user_id, current_month)
        
        # Get limits
        limits = USER_TIERS[tier]
        
        # Check if exceeded
        if usage['tokens'] >= limits['monthly_tokens']:
            self.send_alert(user_id, 'LIMIT_EXCEEDED', usage, limits)
            return {
                'error': 'Monthly token limit exceeded',
                'usage': usage,
                'limits': limits
            }
        
        # Check alert thresholds
        usage_pct = usage['tokens'] / limits['monthly_tokens']
        for threshold in ALERT_THRESHOLDS:
            if usage_pct >= threshold and not usage.get(f'alert_{threshold}_sent'):
                self.send_alert(user_id, f'THRESHOLD_{int(threshold*100)}', usage, limits)
                self.mark_alert_sent(user_id, current_month, threshold)
        
        # Invoke Bedrock
        response = self.invoke_bedrock(prompt)
        
        # Update usage
        self.update_usage(user_id, current_month, response['tokens'], response['cost'])
        
        return response
    
    def get_usage(self, user_id, month):
        response = self.table.get_item(
            Key={'user_id': user_id, 'month': month}
        )
        return response.get('Item', {'tokens': 0, 'cost_usd': 0})
    
    def send_alert(self, user_id, alert_type, usage, limits):
        message = f"""
        Alert: {alert_type}
        User: {user_id}
        Current Usage: {usage['tokens']:,} tokens (${usage['cost_usd']:.2f})
        Limit: {limits['monthly_tokens']:,} tokens (${limits['monthly_cost']:.2f})
        Percentage: {(usage['tokens']/limits['monthly_tokens']*100):.1f}%
        """
        
        self.sns.publish(
            TopicArn='arn:aws:sns:ap-southeast-7:123456789012:bedrock-alerts',
            Subject=f'Bedrock Usage Alert: {alert_type}',
            Message=message
        )
```

---

## Dashboard & Reporting

### Real-Time Usage Dashboard

**CloudWatch Dashboard JSON:**

```json
{
  "widgets": [
    {
      "type": "metric",
      "properties": {
        "metrics": [
          ["Bedrock/Usage", "TokensUsed", {"stat": "Sum"}],
          [".", "Cost", {"stat": "Sum", "yAxis": "right"}]
        ],
        "period": 3600,
        "stat": "Sum",
        "region": "ap-southeast-7",
        "title": "Bedrock Usage - Last 24 Hours"
      }
    },
    {
      "type": "metric",
      "properties": {
        "metrics": [
          ["Bedrock/Usage", "TokensUsed", {"dimensions": {"UserId": "john-doe"}}],
          ["...", {"dimensions": {"UserId": "jane-smith"}}]
        ],
        "period": 86400,
        "stat": "Sum",
        "region": "ap-southeast-7",
        "title": "Top Users by Token Usage"
      }
    }
  ]
}
```

### Monthly Cost Report

**Lambda function to generate monthly reports:**

```python
import boto3
from datetime import datetime
import csv

def generate_monthly_report(event, context):
    dynamodb = boto3.resource('dynamodb')
    s3 = boto3.client('s3')
    table = dynamodb.Table('user_token_usage')
    
    current_month = datetime.now().strftime('%Y-%m')
    
    # Scan all users for current month
    response = table.query(
        IndexName='month-index',
        KeyConditionExpression='month = :month',
        ExpressionAttributeValues={':month': current_month}
    )
    
    # Generate CSV
    csv_data = []
    for item in response['Items']:
        csv_data.append({
            'User ID': item['user_id'],
            'Tokens Used': item['tokens'],
            'Cost (USD)': item['cost_usd'],
            'Tier': item.get('tier', 'basic')
        })
    
    # Upload to S3
    filename = f'bedrock-usage-{current_month}.csv'
    # ... write CSV and upload
    
    return {'statusCode': 200, 'body': f'Report generated: {filename}'}
```

---

## Best Practices

### 1. Set Conservative Limits Initially
- Start with lower limits
- Monitor actual usage
- Adjust based on patterns

### 2. Implement Graceful Degradation
- Don't hard-block users immediately
- Show warnings at 80%, 90%
- Provide self-service upgrade options

### 3. Cache Responses
- Cache common queries
- Reduce duplicate API calls
- Save costs and tokens

### 4. Use Smaller Models When Possible
- Claude Haiku for simple tasks ($0.00025/1K tokens)
- Claude Sonnet for complex tasks ($0.003/1K tokens)
- Automatic model selection based on complexity

### 5. Monitor and Optimize
- Weekly usage reviews
- Identify high-cost users
- Optimize prompts to reduce tokens

---

## Cost Estimation Tool

### Simple Calculator

```python
def estimate_monthly_cost(
    users: int,
    conversations_per_user_per_day: int,
    avg_tokens_per_conversation: int,
    model: str = 'claude-3-sonnet'
):
    # Pricing per 1K tokens
    pricing = {
        'claude-3-haiku': {'input': 0.00025, 'output': 0.00125},
        'claude-3-sonnet': {'input': 0.003, 'output': 0.015},
        'claude-3-opus': {'input': 0.015, 'output': 0.075}
    }
    
    # Assume 40% input, 60% output
    input_tokens = avg_tokens_per_conversation * 0.4
    output_tokens = avg_tokens_per_conversation * 0.6
    
    # Cost per conversation
    cost_per_conv = (
        (input_tokens / 1000) * pricing[model]['input'] +
        (output_tokens / 1000) * pricing[model]['output']
    )
    
    # Monthly cost
    conversations_per_month = users * conversations_per_user_per_day * 30
    monthly_cost = conversations_per_month * cost_per_conv
    
    return {
        'users': users,
        'conversations_per_month': conversations_per_month,
        'cost_per_conversation': round(cost_per_conv, 4),
        'monthly_cost': round(monthly_cost, 2),
        'cost_per_user': round(monthly_cost / users, 2)
    }

# Example
result = estimate_monthly_cost(
    users=100,
    conversations_per_user_per_day=50,
    avg_tokens_per_conversation=500,
    model='claude-3-sonnet'
)
print(result)
# Output:
# {
#   'users': 100,
#   'conversations_per_month': 150000,
#   'cost_per_conversation': 0.0054,
#   'monthly_cost': 810.0,
#   'cost_per_user': 8.1
# }
```

---

## Summary

### Yes, You Can Control Costs Per User!

**Available Controls:**
1. ✅ **AWS Budgets**: Account-level alerts and actions
2. ✅ **API Gateway Usage Plans**: Per-key request limits
3. ✅ **Application Token Tracking**: Custom per-user limits
4. ✅ **CloudWatch Alarms**: Real-time monitoring
5. ✅ **Cost Allocation Tags**: Detailed cost attribution

**Recommended Approach:**
- **Layer 1**: AWS Budgets for overall safety net
- **Layer 2**: API Gateway for request throttling
- **Layer 3**: Application-level token tracking (most granular)
- **Layer 4**: Cost allocation tags for reporting

**Implementation Time:**
- Basic setup: 1-2 days
- Full implementation: 1 week
- Testing and refinement: 1 week

**Cost of Implementation:**
- DynamoDB: ~$5/month
- Lambda: ~$10/month
- CloudWatch: ~$5/month
- **Total**: ~$20/month overhead

This gives you complete control over per-user costs with automated alerts and enforcement!
