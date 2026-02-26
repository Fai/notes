---
tags: [best-practices, documentation, review, maintenance]
created: 2025-12-18
updated: 2025-12-18
---

# Quarterly Documentation Review Checklist

Schedule: First week of January, April, July, October

## Review Process

### 1. Content Audit

#### Knowledge Base Articles
- [ ] Review each article for accuracy
- [ ] Update outdated information (AWS service changes, etc.)
- [ ] Check all links are working
- [ ] Verify code examples still work
- [ ] Update `updated` date in frontmatter

#### Best Practices
- [ ] Review checklists for completeness
- [ ] Add new learnings from recent projects
- [ ] Remove deprecated practices
- [ ] Update AWS CLI commands for new versions

### 2. Structure Review

#### Organization
- [ ] Check for duplicate content
- [ ] Merge similar articles
- [ ] Split overly long articles
- [ ] Verify folder structure makes sense

#### Navigation
- [ ] Update `_index.md` files
- [ ] Update main `README.md`
- [ ] Check cross-references are valid
- [ ] Review Obsidian graph for orphaned notes

### 3. Customer Folder Cleanup

#### Active Projects
- [ ] Ensure READMEs are current
- [ ] Extract any new reusable knowledge
- [ ] Update cross-references

#### Completed Projects
- [ ] Archive old projects (move to `archive/`)
- [ ] Extract final learnings to knowledge-base
- [ ] Remove sensitive information

### 4. Template Review

- [ ] Templates still match current needs
- [ ] Add new templates if patterns emerged
- [ ] Update placeholder text

### 5. Metrics Review

#### Usage (if tracking)
- [ ] Most accessed articles
- [ ] Search terms with no results
- [ ] Feedback received

#### Growth
- [ ] Articles added this quarter
- [ ] Articles updated this quarter
- [ ] New tags added

### 6. Version & Changelog

- [ ] Update CHANGELOG.md with quarterly summary
- [ ] Create git tag if significant changes
- [ ] Push changes to remote

## Quarterly Review Template

```markdown
# Q[X] 2025 Documentation Review

**Date:** YYYY-MM-DD
**Reviewer:** [Name]

## Summary
- Articles reviewed: X
- Articles updated: X
- Articles added: X
- Articles archived: X

## Key Updates
1. 
2. 
3. 

## Action Items for Next Quarter
- [ ] 
- [ ] 

## Notes

```

## Schedule Reminders

Set calendar reminders for:
- **Week 1**: Start review
- **Week 2**: Complete updates
- **Week 3**: Create version tag
- **Week 4**: Share summary with team (if applicable)

## Quick Commands

```bash
# Find articles not updated in 90+ days
find ~/sandbox/docs/knowledge-base -name "*.md" -mtime +90

# Count articles by folder
find ~/sandbox/docs/knowledge-base -name "*.md" | wc -l

# List all tags used
grep -rh "^tags:" ~/sandbox/docs/knowledge-base | sort | uniq

# Create quarterly tag
git tag -a docs-v1.X.0 -m "Q[X] 2025 documentation update"
```

## Related

- [Knowledge Extraction Checklist](knowledge-extraction-checklist.md)
- [CHANGELOG](/CHANGELOG.md)
