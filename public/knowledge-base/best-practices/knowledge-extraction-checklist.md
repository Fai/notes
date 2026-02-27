---
title: "Knowledge Extraction Checklist"
tags: [best-practices, documentation, knowledge-management]
created: 2025-12-18
updated: 2025-12-18
---
title: "Knowledge Extraction Checklist"

# Knowledge Extraction Checklist

Use this checklist when closing a customer engagement to capture reusable knowledge.

## Before Engagement Ends

### 1. Identify Reusable Content

Review all documents created during the engagement:

- [ ] Troubleshooting guides
- [ ] Architecture decisions
- [ ] Configuration templates
- [ ] Scripts and automation
- [ ] Incident reports
- [ ] Performance tuning notes

### 2. Categorize by Type

| Content Type | Extract To | Example |
|--------------|------------|---------|
| AWS service troubleshooting | `/knowledge-base/aws/` | RDS connectivity issues |
| Kubernetes patterns | `/knowledge-base/kubernetes/` | Node management |
| General troubleshooting | `/knowledge-base/troubleshooting/` | Database connections |
| Operational patterns | `/best-practices/` | Incident response |
| Reusable templates | `/_templates/` | Runbook template |

### 3. Extract Knowledge

For each piece of reusable content:

- [ ] Remove customer-specific details (names, IPs, account IDs)
- [ ] Generalize the solution
- [ ] Add context for when to use
- [ ] Include diagnostic commands
- [ ] Add related links

### 4. Create Knowledge Article

Use the template: `/_templates/knowledge-article.md`

Required sections:
- [ ] Frontmatter with tags
- [ ] Overview/when to use
- [ ] Quick reference table
- [ ] Detailed steps
- [ ] Common issues
- [ ] Related articles

### 5. Update Customer README

Add cross-references to extracted knowledge:

```markdown
## Related Knowledge Base Articles

- [Article Name](/knowledge-base/category/article.md)
```

## After Extraction

### 6. Update Indexes

- [ ] Update `/knowledge-base/_index.md`
- [ ] Update `/best-practices/_index.md` (if applicable)
- [ ] Update `/docs/README.md` (if major addition)

### 7. Update CHANGELOG

Add entry to `/docs/CHANGELOG.md`:

```markdown
## [YYYY-MM-DD]

### Added
- knowledge-base/aws/new-article.md - Description
```

### 8. Review Quality

- [ ] Article is self-contained (can be understood without customer context)
- [ ] Commands are tested and work
- [ ] Links are valid
- [ ] Tags are appropriate
- [ ] Frontmatter is complete

## Quick Extraction Questions

Ask yourself:

1. **Did I solve a problem that could happen again?** → Extract to knowledge-base
2. **Did I create a process that others could follow?** → Extract to best-practices
3. **Did I create a reusable template?** → Add to _templates
4. **Did I learn something about an AWS service?** → Add to knowledge-base/aws

## Example Extraction

### Before (Customer-specific)
```
Fixed RDS connection issue for Acme Corp.
Their Fargate tasks couldn't connect to RDS.
IP: 10.0.1.50, DB: acme-prod-db
Solution: Added SSL parameter to connection string.
```

### After (Generalized)
```markdown
# RDS Connectivity Troubleshooting

## SSL Configuration Issue

**Symptoms:**
- Fargate tasks fail to connect to RDS
- Error: "SSL required"

**Solution:**
Add `sslmode=require` to connection string:
postgresql://user:pass@endpoint:5432/db?sslmode=require
```

## Related

- [Knowledge Article Template](/_templates/knowledge-article.md)
- [Customer README Template](/_templates/customer-readme.md)
