---
title: "Nosto vs AWS Recommendation Services — Comparison"
tags: [research, aws, personalization, nosto, recommendation, presales, kadokawa]
created: 2026-02-26
status: complete
---

# Nosto vs AWS Recommendation Services — Comparison

> Context: Prepared for KADOKAWA AMARIN (Phoenix Next) discovery meeting 2026-02-26.
> Customer is evaluating AWS as a cost-reduction alternative to Nosto.

---

## Nosto Overview

**Nosto** is a Commerce Experience Platform (CXP) providing turnkey product recommendations, personalized content, pop-ups, and A/B testing built specifically for e-commerce. No ML expertise required — integrates via embedded JS tag on Shopify, Magento, or custom storefronts.

**Pricing model (current):** Fixed monthly fee based on merchant revenue/GMV tier. Originally pay-per-performance (commission on influenced revenue), transitioned to flat SaaS fee ~2017–2018. Scales with merchant size — becomes significant cost at mid-market GMV.

---

## Head-to-Head Comparison

| Dimension | Nosto | Amazon Personalize | Bedrock + Knowledge Bases |
|-----------|-------|--------------------|--------------------------|
| **Setup effort** | Very low (JS tag) | Low–Medium (data pipeline) | Low |
| **Target user** | Marketer / merchandiser | Developer / ML team | Developer |
| **Recommendation type** | Product recs, upsell, cross-sell, trending | Collaborative filtering, user personalization | Generative / reasoning-based |
| **E-commerce native** | ✅ Built-in catalog, cart, order events | ❌ Must build event ingestion | ❌ Must build |
| **A/B testing** | ✅ Built-in | ❌ Must build at app/CDN layer | ❌ Must build |
| **Merchandising rules** | ✅ Drag-and-drop (pin, exclude, boost) | ⚠️ Filter expressions only | ❌ Manual |
| **Real-time behavioral** | ✅ Yes | ✅ Yes (Event Tracker API) | ❌ No |
| **Data ownership** | ❌ Vendor holds data | ✅ Your AWS account | ✅ Your AWS account |
| **Customization** | Low | High | Very high |
| **Cost model** | SaaS / GMV-based (scales with revenue) | Pay-per-use (~$20–50/mo at 100K MAU) | Pay-per-token |
| **Vendor lock-in** | High | Medium (AWS) | Medium (AWS) |
| **Cold start** | ✅ Handles well (popularity fallback) | ⚠️ Needs interaction history | ⚠️ Needs content metadata |

---

## When AWS Wins

- Customer wants **data sovereignty** — all behavioral data stays in their AWS account
- Already on AWS — opportunity to consolidate vendors and reduce SaaS spend
- **Cost at scale** — Nosto GMV-based fee grows with revenue; Personalize is usage-based and predictable
- Needs **custom recommendation logic** beyond standard e-commerce patterns
- Has a dev team willing to own the pipeline

## When Nosto Still Wins

- Small team, no ML/data engineering capacity
- Needs **merchandising UI** for non-technical staff (editors, marketers)
- Wants **out-of-the-box A/B testing** on recommendation placements
- Fast time-to-value (days vs weeks for AWS setup)

---

## Migration Path: Nosto → Amazon Personalize

```
Nosto interaction data (orders, views, clicks)
    → export to S3 (CSV: userId, itemId, eventType, timestamp)
    → Personalize Dataset Group
        ├── Users dataset (userId, demographics)
        ├── Items dataset (itemId, genre, author, price, etc.)
        └── Interactions dataset (userId, itemId, eventType, timestamp)
    → Train recipe: aws-user-personalization
    → Deploy real-time campaign endpoint
    → Replace Nosto JS widget with API call
    → A/B split at app layer (feature flag or CloudFront)
```

### Key Gaps to Fill

| Nosto Feature | AWS Equivalent |
|---------------|---------------|
| Recommendation carousel widget | Custom frontend component calling Personalize API |
| Merchandising rules UI | Custom admin panel or filter expressions in API call |
| Built-in A/B testing | CloudFront + Lambda@Edge traffic split, or feature flag service |
| Popularity fallback (cold start) | Personalize `popularity-count` recipe or manual trending list |
| Email recommendation | Personalize batch inference → SES |

---

## Cost Estimate — Phoenix Next Scale

| Component | Monthly Est. |
|-----------|-------------|
| Personalize training (1×/month, 200K interactions) | ~$5–15 |
| Personalize inference (200K req/mo @ $0.0417/1K) | ~$8 |
| S3 storage + data pipeline | ~$5 |
| **Total AWS** | **~$20–30/month** |

Nosto equivalent at this scale: custom quote, typically $500–$2,000+/month for mid-market merchants depending on GMV tier.

**Estimated savings: 90%+ reduction in recommendation infrastructure cost.**

---

## POC Recommendation for KADOKAWA

**Approach:** 4-week A/B test — 50% traffic to Nosto (control), 50% to Personalize (treatment)

**Success metrics:**
- Click-through rate on recommendation carousels
- Add-to-cart rate from recommendations
- Conversion rate (recommendation → purchase)
- Revenue per session

**Minimum data requirement:** 6+ months of interaction history (views, purchases) for Personalize to train effectively.

---

## Related

- [[research/aws/aws-recommendation-personalization-services-2025|AWS Recommendation & Personalization Services — Pre-Sales Brief]]
- [[journal/daily/2026-02-26-kadokawa-meeting|KADOKAWA Meeting Note 2026-02-26]]
