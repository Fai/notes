---
title: "Azure to AWS Migration Checklist"
tags: [best-practices, migration, azure, aws, checklist]
created: 2025-12-19
updated: 2025-12-19
---
title: "Azure to AWS Migration Checklist"

# Azure to AWS Migration Checklist

Application-level checklist for migrating from Azure to AWS.

## 1. Compute & Container Migration

- [ ] Map Azure Container Instances/App Service → **ECS Fargate**
- [ ] Update container registry: Azure Container Registry → **ECR**
- [ ] Adjust health check endpoints and timeouts for ALB
- [ ] Update CPU/memory configurations for Fargate task definitions

## 2. Database Migration

### PostgreSQL
- [ ] Azure Database for PostgreSQL → **RDS PostgreSQL**
- [ ] Export schema and data using `pg_dump`
- [ ] Configure Multi-AZ for production
- [ ] Update connection strings

### MongoDB
- [ ] Azure Cosmos DB → **DocumentDB** or **MongoDB Atlas**
- [ ] Export collections and migrate data
- [ ] Update MongoDB connection URIs

### Redis
- [ ] Azure Cache for Redis → **ElastiCache Redis**
- [ ] Sessions and rate limiting will need reconnection
- [ ] No data migration needed (ephemeral cache)

## 3. Application Configuration

- [ ] Update config files with AWS endpoints
- [ ] Replace Azure Key Vault → **Secrets Manager** or **Parameter Store**
- [ ] Update environment variables:
  - Database endpoints
  - Redis cluster endpoint
  - S3 bucket names
  - OAuth callback URLs

## 4. Storage Migration

- [ ] Azure Blob Storage → **S3**
- [ ] Migrate files using `aws s3 sync` or AWS DataSync
- [ ] Update presigned URL generation code
- [ ] Update CDN: Azure CDN → **CloudFront**

## 5. Networking & Security

- [ ] Configure VPC, subnets, security groups
- [ ] Set up **WAF** rules
- [ ] Update SSL certificates in **ACM**
- [ ] Configure DNS in **Route 53**

## 6. Authentication & OAuth

- [ ] Update OAuth redirect URIs for all platforms
- [ ] Migrate secrets to Secrets Manager
- [ ] Test OAuth flows end-to-end

## 7. CI/CD Pipeline

- [ ] Update deployment scripts for AWS CLI
- [ ] Configure **CodePipeline** or update GitHub Actions for ECR/ECS
- [ ] Update build scripts for ECR push

## 8. Monitoring & Logging

- [ ] Azure Monitor → **CloudWatch**
- [ ] Set up CloudWatch Logs for container logs
- [ ] Configure alarms for API latency, error rates
- [ ] Update application logging configuration

## 9. Testing

- [ ] Run integration tests against new infrastructure
- [ ] Verify WebSocket connections through ALB
- [ ] Test all critical user flows
- [ ] Load test to verify performance parity

## 10. Cutover Preparation

- [ ] Reduce DNS TTL before migration
- [ ] Prepare rollback procedure
- [ ] Schedule maintenance window
- [ ] Final data sync before cutover

---
title: "Azure to AWS Migration Checklist"

## Service Mapping Reference

| Azure Service | AWS Service |
|---------------|-------------|
| Azure Container Instances | ECS Fargate |
| Azure Container Registry | ECR |
| Azure Database for PostgreSQL | RDS PostgreSQL |
| Azure Cosmos DB | DocumentDB / MongoDB Atlas |
| Azure Cache for Redis | ElastiCache Redis |
| Azure Blob Storage | S3 |
| Azure CDN | CloudFront |
| Azure Key Vault | Secrets Manager / Parameter Store |
| Azure Monitor | CloudWatch |
| Azure DNS | Route 53 |
| Azure Application Gateway | ALB + WAF |
| Azure Functions | Lambda |
| Azure Service Bus | SQS / SNS |
| Azure Event Grid | EventBridge |

## Related

- [Migration Checklist](./migration-checklist.md) - Generic migration checklist
- [Security Checklist](./security-checklist.md)
