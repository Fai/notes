---
title: AWS Recommendation and Personalization Services — Pre-Sales Technical Brief
tags: [research, aws, personalization, recommendation, amazon-personalize, bedrock, presales]
created: 2026-02-26
status: complete
---

# AWS Recommendation and Personalization Services — Pre-Sales Technical Brief

> Research compiled: 2026-02-26. Pricing and limits sourced from AWS documentation. Use for customer conversations — verify against AWS pricing calculator for final quotes.

---

## Quick Reference Summary

| Service | Best For | Effort | Cost Entry Point |
|---------|----------|--------|-----------------|
| Amazon Personalize | Purpose-built collaborative filtering, behavioral recommendations | Low-Medium | ~$5–50/month (low volume) |
| Amazon Bedrock + Knowledge Bases | Generative/reasoning-based personalization, content variation | Low | Per-token model cost |
| Bedrock Agents | Conversational recommendation, multi-step personalized workflows | Medium | Per-token + orchestration |
| OpenSearch k-NN | Vector similarity search, custom embedding-based recommendations | Medium | Instance/OCU-based |
| Amazon Kendra GenAI | Personalized enterprise search | Low-Medium | $0.32/hr base index |
| SageMaker | Fully custom recommendation models (two-tower, matrix factorization) | High | Instance-based |

---

## 1. Amazon Personalize — Deep Dive

### 1.1 What It Is

Amazon Personalize is a **fully managed ML recommendation service** that trains custom models on your behavioral data (clicks, views, purchases, ratings) to generate personalized item recommendations, user segments, and next-best-action recommendations. No ML expertise required. AWS manages all infrastructure.

**Core value proposition:** Go from data to working recommendation API in hours instead of months. The service implements industry-proven collaborative filtering algorithms (HRNN, Bandits, Transformer-based) behind an abstracted API.

### 1.2 Use Cases

- **E-commerce:** "Recommended for you," "Frequently bought together," "Customers who viewed X also viewed," trending products
- **Video/Streaming:** "Top picks for you," "More like X," "Most popular," trending content
- **Marketing:** Personalized email content, user segmentation for targeting campaigns
- **In-app:** Next-best-action (e.g., recommend loyalty enrollment, app feature discovery)
- **Search personalization:** Re-ranking search results per user (OpenSearch integration)
- **Anonymous/session users:** Real-time personalization before login using session ID

**Proven outcomes from customers:**
- Seven West Media: 48% increase in minutes viewed, 3x viewer interaction
- Discovery Education: 229% click-through rate increase
- FOX Corporation: 6% lift in avg minutes viewed per recommendation, 15% bounce rate reduction
- Bundesliga: 67% increase in article reads per user, 17% longer sessions
- Razer: Click-through rates 10x above industry standard
- Equinox: 92% engagement increase with carousel content

### 1.3 How It Works — Core Concepts

#### Dataset Groups
The top-level container. Two types:
- **Domain Dataset Group** — Pre-configured for `VIDEO_ON_DEMAND` or `ECOMMERCE`. Comes with use-case optimized recommenders. Recommended starting point.
- **Custom Dataset Group** — Full flexibility, configure your own recipes and solutions. Required for use cases outside the two domains (e.g., news, music, travel, gaming).

#### Datasets
Three core dataset types (all use CSV format uploaded to S3):

| Dataset | Required Fields | Optional Fields |
|---------|----------------|----------------|
| Item Interactions | USER_ID, ITEM_ID, TIMESTAMP | EVENT_TYPE, EVENT_VALUE, contextual metadata |
| Items | ITEM_ID | CREATION_TIMESTAMP, categorical/textual metadata (genre, price, description) |
| Users | USER_ID | Demographic metadata (age, gender, membership_level) |

For Next-Best-Action: also uses **Actions** and **Action Interactions** datasets.

#### Recipes (Algorithms)
A "recipe" is a pre-defined ML algorithm trained on your data. You select a recipe, configure hyperparameters (or use AutoML), and Personalize trains a **Solution Version** (trained model artifact).

#### Solutions and Solution Versions
- **Solution** = configuration (recipe + hyperparameters)
- **Solution Version** = a trained model. You can have multiple solution versions per solution (e.g., retrain weekly)
- Training takes minutes to hours depending on data volume

#### Campaigns (Custom Resources)
- Deploys a solution version with provisioned capacity for **real-time** recommendations
- Minimum provisioned TPS = 1 (default; this is the billing floor — you pay even with zero traffic)
- Auto-scales above minProvisionedTPS, then scales back down
- **Note:** There is a short delay during scale-up that can cause dropped transactions — set minProvisionedTPS appropriately for your peak if latency matters

#### Recommenders (Domain Dataset Groups)
- Pre-built, managed recommenders for the two supported domains
- **Automatically retrain** on a schedule (no manual solution version management)
- Simpler to operate than custom campaigns

### 1.4 Recipes — Complete List

#### USER_PERSONALIZATION (Personalized recommendations for each user)
| Recipe | Status | Notes |
|--------|--------|-------|
| **User-Personalization-v2** | Current (recommended) | Up to 5M items, lower latency, faster training vs v1 |
| User-Personalization | Legacy | Still supported |
| HRNN | Legacy | Hierarchical RNN, deprecated |
| HRNN-Metadata | Legacy | |
| HRNN-Coldstart | Legacy | |

**User-Personalization-v2 key improvements over v1:**
- Scales to **5 million items** (v1 capped at 750,000)
- **Faster training** and **lower inference latency**
- Supports real-time event personalization (via PutEvents API, recommendations adapt within seconds)
- Exploration/exploitation via contextual bandits for new item discovery

#### POPULAR_ITEMS (Trending and popular)
| Recipe | Notes |
|--------|-------|
| **Trending-Now** | Time-windowed trending; configurable recency window |
| **Popularity-Count** | All-time popularity ranking |

#### PERSONALIZED_RANKING (Re-rank a curated list per user)
| Recipe | Notes |
|--------|-------|
| **Personalized-Ranking-v2** | Current (recommended); lower latency |
| Personalized-Ranking | Legacy |

Use case: You have a list of 50 items from a search/editorial team — this re-orders them specifically for each user. Critical for **search personalization** (OpenSearch integration).

#### RELATED_ITEMS (Similar item recommendations)
| Recipe | Notes |
|--------|-------|
| **Similar-Items** | Combines collaborative filtering + item content (text/metadata) |
| **Semantic-Similarity** | Pure content/embedding-based (no interaction data needed — good for new catalogs) |
| SIMS | Legacy collaborative filtering only |

**Similar-Items vs Semantic-Similarity:**
- Similar-Items: needs interaction history + can use item metadata text
- Semantic-Similarity: works from item content alone (zero interaction data) — useful for cold catalogs or when behavioral data is sparse

#### PERSONALIZED_ACTIONS (Next-Best-Action)
| Recipe | Notes |
|--------|-------|
| **Next-Best-Action** | Recommends which action a user should take next (e.g., sign up for loyalty, download app) |

Requires: Actions dataset + Action Interactions dataset. Models the probability a user will complete a specific action based on their history.

#### USER_SEGMENTATION (Batch; marketing campaigns)
| Recipe | Notes |
|--------|-------|
| **Item-Affinity** | Creates a user segment (list of user IDs) who have affinity for a specific item |
| **Item-Attribute-Affinity** | Creates user segments based on item attribute affinity (e.g., "users who like action movies") |

Output: user ID lists, typically fed into SES, Pinpoint, or a CDP.

### 1.5 Domain Recommenders (Pre-built Use Cases)

#### VIDEO_ON_DEMAND Domain
| Use Case | What It Does |
|----------|-------------|
| Top picks for you | Personalized recommendations per user |
| More like X | Items similar to a given item |
| Most popular | Popularity-based (no user ID needed) |
| Because you watched X | Content similar to a specific watched item |
| Trending now | Recently trending content |

#### ECOMMERCE Domain
| Use Case | What It Does |
|----------|-------------|
| Recommended for you | Full catalog personalized recommendations |
| Frequently bought together | Co-purchase patterns |
| Customers who viewed X also viewed | Co-view patterns |
| Best sellers | Popularity-based |
| Most viewed | View-count based |

### 1.6 Real-Time vs Batch

#### Real-Time
- **API:** `GetRecommendations`, `GetPersonalizedRanking`, `GetActionRecommendations`
- **Latency:** Single-digit milliseconds to ~50ms (P99 varies by catalog size)
- **Updates:** PutEvents API streams user interactions; recommendations adapt **within seconds** for supported recipes (User-Personalization-v2, Personalized-Ranking-v2, Next-Best-Action)
- **Anonymous users:** Pass `sessionId` as `userId` before login; merge on login
- **Contextual metadata:** Pass device, time, location at inference time (must be in schema)
- **Filtering:** Apply filter expressions at query time (e.g., exclude purchased items, filter by category)

#### Batch
- **Use case:** Email personalization, bulk recommendation pre-computation, offline scoring
- **No campaign needed** for batch jobs
- **Input:** S3 JSON file with user IDs
- **Output:** S3 JSON file with recommendations per user
- **Latency:** Job-based, not real-time (minutes to hours depending on volume)
- **Max input per batch job:** 1 GB, 1,000 input files, 50 million records (without themes)

### 1.7 Contextual Recommendations

Pass context at inference time to shift recommendations based on current session:

```python
response = personalizeRt.get_recommendations(
    campaignArn='arn:...',
    userId='user123',
    context={
        'DEVICE': 'mobile',
        'TIME_OF_DAY': 'evening'
    }
)
```

Context fields must exist in the Item Interactions schema. Supported recipes: User-Personalization-v2/v1, Personalized-Ranking-v2/v1, ECOMMERCE "Recommended for you," VIDEO_ON_DEMAND "Top picks for you."

### 1.8 Filtering

Real-time and batch both support filter expressions. Up to 30 filters per dataset group.

```
# Example filter: exclude already-purchased items
EXCLUDE ItemID WHERE INTERACTIONS.event_type IN ("purchase")

# Filter by item attribute
INCLUDE ItemID WHERE ITEMS.genre IN ("action","thriller")
```

Filter limits:
- Max 10 distinct dataset fields per filter
- Max 20 distinct fields across all filters in a dataset group
- Max 100 interactions per user/event type considered in filters (adjustable)

### 1.9 Data Requirements and Constraints

#### Absolute Minimums (to train a model)
- **1,000 unique interactions** (records in item interactions dataset)
- **25 unique users** with at least 2 interactions each
- At least **1% of users** must have 2+ interactions

#### Practical Minimums for Good Quality
- **50,000+ interactions** recommended for meaningful recommendations
- **Diverse user behavior** — if most users have 1–2 interactions, quality suffers
- **Temporal spread** — all interactions from one day is worse than interactions over weeks

#### Scale Maximums
| Dimension | User-Personalization-v2 / Personalized-Ranking-v2 | Other Recipes |
|-----------|---------------------------------------------------|--------------|
| Items | 5,000,000 | 750,000 |
| Interactions | 3,000,000,000 | 500,000,000 |
| Users | No documented hard limit | — |

#### Dataset Limits
| Type | Full Import Max | Incremental Import Max |
|------|----------------|----------------------|
| Item Interactions | 100 GB | 1 GB |
| Items | 100 GB | 1 GB |
| Users | 100 GB | 1 GB |

#### API Rate Limits
| API | Limit |
|-----|-------|
| PutEvents | 1,000 calls/sec per dataset group |
| PutItems | 10 calls/sec per dataset group |
| PutUsers | 10 calls/sec per dataset group |
| GetRecommendations | 500 TPS per campaign |
| GetPersonalizedRanking | 500 TPS per campaign |
| Combined recommendation TPS | 2,500/sec total |

**Max recommendations returned per call:**
- Without metadata: 500 items
- With metadata: 50 items

### 1.10 Cold Start Problem

This is the most common gotcha. Two dimensions:

#### New User Cold Start
- User has zero interaction history
- Personalize falls back to **popularity-based recommendations**
- Solution: Collect at least one interaction (click/view) and use PutEvents in real-time — recommendations personalize within the same session
- User-Personalization-v2 uses a **contextual bandit exploration** strategy to explore new recommendations even for sparse users

#### New Item Cold Start
- Item added to catalog with no interactions
- **Item Exploration:** User-Personalization-v2 has a configurable **exploration weight** (0–1) that controls how aggressively new items are surfaced to gather interaction data
- **Semantic-Similarity recipe** works from item content alone — useful for new items
- **Similar-Items recipe** uses item text/metadata to cold-start related item recommendations
- **CREATION_TIMESTAMP field** on items helps — Personalize uses it to identify "new" items and boosts their exploration

#### Minimum Data to Train at All
- Below 1,000 interactions: cannot create solution version
- Workaround options: use Bedrock/LLM-based recommendations, use popularity-count recipe, use Semantic-Similarity (content-only)

### 1.11 Pricing

#### Data Ingestion
- **$0.05 per GB** of data uploaded (S3 batch or real-time stream)

#### Training
- **$0.002 per 1,000 interactions** considered during training
- Example: 5M interactions = $10 per training run

#### Real-Time Recommendations
- **$0.15 per 1,000 recommendation requests**
- Minimum charge: **1 TPS provisioned** per active campaign, billed continuously
- Auto-scaling charges: billed for whichever is higher — provisioned minimum or actual usage

#### Batch Recommendations
- **$0.15 per 1,000 recommendation requests** (same rate as real-time)

#### Free Tier (First 2 Months for New Users)
- 20 GB data processing/month
- 5 million training interactions/month (100 training hours)
- 50,000 real-time requests/month (v2 recipes)
- Note: AWS marketing mentions 180,000 real-time recommendations/month in some places — verify current free tier terms

#### Cost Example at Scale

| Scenario | Monthly Cost Estimate |
|----------|-----------------------|
| Small app: 100K interactions, 1 campaign (1 TPS min), 100K rec requests/month | ~$20–30 |
| Medium: 5M interactions, 2 campaigns (5 TPS each), 10M requests/month | ~$2,500 |
| Large: 500M interactions, 10 campaigns (20 TPS each), 100M requests/month | ~$20,000+ |

**Key cost trap:** Every active campaign charges for the minProvisionedTPS floor **24/7**. At $0.15/1000 × 3,600 × 24 × 30 = **~$388/month per TPS provisioned** even with zero traffic. Delete unused campaigns.

### 1.12 Recent Updates (2024–2025)

Based on available documentation:
- **User-Personalization-v2** — Generally available; 5M item support, lower latency, faster training
- **Personalized-Ranking-v2** — Updated version of ranking recipe
- **Semantic-Similarity recipe** — Content-based recommendations without interaction data
- **Bedrock integration** — Amazon Personalize + Bedrock for AI-generated content variations (marketing copy, descriptions personalized per segment)
- **SageMaker AI Data Wrangler integration** — 40+ data source connectors for data prep
- **Metric Attribution** — Track recommendation effectiveness across channels (up to 10 metrics, 100 attribution sources)

---

## 2. Amazon Bedrock for Personalization

### 2.1 The Paradigm Shift

Bedrock does not replace Personalize — it **complements** it. The key distinction:

| | Amazon Personalize | Amazon Bedrock |
|-|-------------------|---------------|
| Approach | Collaborative filtering on behavioral data | Foundation model reasoning |
| Strength | Scales to billions of interactions, millisecond latency | Reasoning, language understanding, content generation |
| Data needed | User interaction history | Prompt context, documents, structured data |
| Personalization type | "Users like you liked X" | "Given your stated preferences and context, I recommend X because..." |
| Cost model | Per-request, per-TPS | Per-token |

### 2.2 Foundation Models for Recommendation Reasoning

Use Bedrock FMs when you need **explainable, language-aware, or reasoning-based** recommendations:

**Use cases:**
- "Given this user's browsing history and stated preference for minimalist design, which of these 10 products best matches their style?" (FM re-ranks a candidate list)
- Generating **personalized explanations** for why a product was recommended
- Handling **zero-shot new category** recommendations where behavioral data doesn't exist
- **Conversational recommendations** ("I'm looking for a birthday gift for my mother who likes gardening")

**Model choices for recommendation reasoning:**
- **Claude 3.5 Sonnet:** $3/M input tokens, $15/M output tokens — best for complex reasoning
- **Claude 3 Haiku:** $0.25/M input tokens, $1.25/M output tokens — fast, cheap for simple re-ranking
- **Amazon Nova Pro:** Cheaper AWS-native option
- **Llama 3:** Open-source option via Bedrock

**Pattern: FM as re-ranker**
```
1. Get 50 candidate items from Personalize or search
2. Send top candidates + user profile to Bedrock FM
3. FM reasons about best match and returns ordered list + explanation
4. Return top 5 with personalized explanations to user
```

### 2.3 Knowledge Bases + RAG for Personalized Content

**What it is:** Bedrock Knowledge Bases connects your private documents/content to an FM via RAG. The FM generates responses grounded in your content, with citations.

**How it works:**
1. Documents chunked and embedded (Titan Embeddings V2, Cohere Embed, etc.)
2. Embeddings stored in vector store
3. At query time: embed user query, retrieve relevant chunks, pass to FM
4. FM generates grounded response

**Supported vector stores:**
- Amazon OpenSearch Serverless (auto-provisioned, easiest)
- Amazon Aurora PostgreSQL (pgvector)
- Pinecone, MongoDB Atlas, Redis, Weaviate (via connector)
- Amazon Neptune Analytics (graph-based retrieval)
- Amazon Kendra GenAI indices (semantic search)

**Supported data sources:** S3, Confluence, SharePoint, Salesforce, web crawl, and more

**Personalization via RAG:**
- Store user profiles, purchase history, preferences as documents
- At query time, retrieve user-specific context and pass to FM
- FM generates personalized response using retrieved context
- Example: Personalized product education content, personalized FAQ answers

**Embedding model pricing (for Knowledge Base setup):**
- Amazon Titan Embeddings V2: ~$0.02 per 1M tokens
- Cohere Embed: ~$0.10 per 1M tokens

**Gotcha:** RAG latency is higher than Personalize (200ms–2s depending on retrieval + FM time). Not suitable for sub-100ms real-time recommendation widgets.

### 2.4 Bedrock Agents for Dynamic Recommendations

**What agents enable:**
- Multi-step personalized workflows (e.g., understand request → check inventory → check user history → generate recommendation → explain reasoning)
- Memory retention across sessions (personalized context accumulates over time)
- Action groups that call external APIs (loyalty system, CRM, inventory)
- Multi-agent collaboration (supervisor + specialist agents for different recommendation domains)

**Architecture pattern — Recommendation Agent:**
```
User: "What should I buy for my upcoming camping trip?"
→ Agent extracts intent (camping gear)
→ Agent queries Knowledge Base (product catalog, reviews)
→ Agent calls Personalize action (get user's past outdoor purchases)
→ Agent calls inventory API (check availability)
→ Agent generates personalized recommendation with reasoning
→ Agent remembers preference for future sessions
```

**When to use Agents vs direct FM:**
- Use direct FM call: single-turn, structured input, latency-sensitive
- Use Agents: multi-turn, multi-step orchestration, need memory, need API calls

### 2.5 Personalize + Bedrock Integration Patterns

#### Pattern A: Personalize Candidates → Bedrock Ranking
1. Personalize returns 50 candidates (fast, behavioral matching)
2. Bedrock FM re-ranks with reasoning (quality signal)
3. Return top 5 with personalized copy

#### Pattern B: Bedrock Content Variations for Personalize Segments
1. Personalize batch segments identify user groups (e.g., "power users who prefer action")
2. Bedrock generates personalized email copy, product descriptions, push notification text for each segment
3. Personalized content delivered via Pinpoint/SES

#### Pattern C: Full GenAI Stack (no Personalize)
For new products or sparse data:
1. User provides natural language preference
2. Bedrock Agent queries Knowledge Base (product catalog)
3. FM reasons about match and explains
4. No interaction history needed — works from day 1

---

## 3. Other AWS Services in the Personalization Stack

### 3.1 Amazon Kendra — Personalized Search

**What it adds:** Enterprise search with semantic understanding, NLP-based question answering, and personalization signals.

**Personalization capabilities:**
- **User behavior analysis** — re-ranks results based on what users click
- **Content attribute filtering** — filter by freshness, relevance score, custom attributes
- **LLM integration** — Kendra + Bedrock for conversational search ("What is our return policy for electronics?")
- **Personalize integration** — Kendra results can be re-ranked by Personalize

**Pricing (us-east-1):**

| Edition | Base Index Cost | Documents | Queries/day |
|---------|----------------|-----------|-------------|
| GenAI Enterprise | $0.32/hr (~$230/month) | 20,000 / 200 MB | ~8,000 (0.1 QPS) |
| Basic Enterprise | $1.40/hr (~$1,008/month) | 100,000 / 30 GB | ~8,000 (0.1 QPS) |
| Developer | $1.125/hr (~$810/month) | 10,000 / 3 GB | ~4,000 (0.05 QPS) |

Additional capacity units:
- GenAI: Storage $0.25/hr, Query $0.07/hr
- Enterprise: $0.70/hr each

Connectors: $30/month flat (GenAI) or $0.35/hr + $1/M documents scanned (others)

**When to recommend Kendra over OpenSearch:**
- Enterprise document search (SharePoint, Confluence, internal KB)
- Need out-of-box NLP/semantic search without ML expertise
- Regulatory/compliance environments needing managed service
- When the content is unstructured documents, not items in a catalog

### 3.2 OpenSearch Service — Vector Search and Personalized Ranking

**Personalization patterns with OpenSearch:**

#### Pattern A: Personalize + OpenSearch Re-ranking
1. User submits search query
2. OpenSearch returns lexical/BM25 results
3. Personalize `GetPersonalizedRanking` re-orders by user preference
4. Personalized results returned to user
- Published AWS integration: this pattern is documented in AWS blog (Feb 2024)

#### Pattern B: k-NN Vector Search for Similarity
- Store item embeddings (from SageMaker or Bedrock Titan Embeddings) as vectors
- At query time: embed user query/preference, find k-nearest neighbors
- Supports Euclidean distance and cosine similarity
- Max vector dimensions: 10,000 floats per vector field
- Max k: 10,000 results
- Memory: k-NN uses up to 50% of RAM after JVM heap

#### Pattern C: Semantic Personalization
- Combine BM25 (lexical) + k-NN (semantic) + Personalize score as a hybrid ranking signal
- OpenSearch supports script scoring to blend multiple signals

**Pricing:**
- Serverless: minimum 2 OCUs (6 GB RAM + vCPU + GP3 + S3 transfer each)
- Vector search collections use dedicated OCU pool (cannot share with regular search)
- GPU-accelerated vector OCUs available for high-throughput indexing
- Reserved instances: up to 35% (1-year), 52% (3-year) discount

### 3.3 SageMaker — Custom Recommendation Models

**When to use SageMaker instead of Personalize:**
- Need full control over model architecture (two-tower networks, DLRM, sequential models)
- Have a large data science team with ML expertise
- Personalize limits (e.g., 5M items, specific schema) are insufficient
- Need to incorporate custom signals not supported by Personalize (visual similarity, graph signals)
- Regulatory requirement to own/audit the model fully

**Common SageMaker recommendation architectures:**
- **Two-Tower Model** — User tower + item tower, dot-product similarity at retrieval
- **Sequential Models** — BERT4Rec, SASRec for session-based recommendations
- **Matrix Factorization** — Classic CF with SparkML on EMR or SageMaker
- **Wide & Deep / DLRM** — For click-through rate prediction (ranking stage)

**SageMaker components typically used:**
- SageMaker Feature Store — Store user/item features for real-time serving
- SageMaker Training — Train model (spot instances for cost savings)
- SageMaker Endpoint — Real-time inference hosting
- SageMaker Batch Transform — Batch recommendations

**Rough cost ranges (us-east-1):**
- Training: ml.p3.2xlarge ~$3.82/hr (8 V100 GPU), ml.m5.4xlarge ~$0.96/hr
- Inference endpoint: ml.m5.xlarge ~$0.23/hr per instance
- Feature Store: $0.00012/write unit, $0.00008/read unit

**Total cost vs Personalize:** At scale, SageMaker can be cheaper per-recommendation but requires significantly more engineering effort (MLOps, monitoring, retraining pipelines).

### 3.4 AWS AppSync + DynamoDB — Recommendation Delivery Layer

**AppSync pricing:**
- Query/mutation operations: $4.00 per million
- Real-time updates (subscriptions): $2.00 per million + $0.08 per million connection-minutes
- Caching (optional): $0.044/hr (small) to $6.775/hr (12xlarge)

**Common patterns for recommendation delivery:**

#### Pattern: Real-Time Recommendation Widget
```
Client → AppSync GraphQL → Lambda resolver
→ Lambda → Amazon Personalize GetRecommendations
→ Lambda → DynamoDB (item metadata lookup)
→ Return enriched recommendations to client
```

#### Pattern: Pre-computed Recommendation Cache
```
Scheduled Lambda → Personalize Batch → S3
→ Lambda → DynamoDB (write recommendations keyed by user_id)
Client → AppSync → DynamoDB (read pre-computed recommendations)
```

**DynamoDB for recommendations:**
- Store pre-computed recommendations (key: user_id, value: list of item_ids)
- Single-digit millisecond reads — ideal as a recommendation cache
- TTL to expire stale recommendations and trigger refresh

**Lambda for orchestration:**
- $0.20 per million requests
- 400,000 GB-seconds free per month
- Graviton2 functions: 34% better price-performance for sustained workloads

---

## 4. Architecture Patterns — Full Personalization Stack

### Pattern 1: Standard Managed Stack (Recommended Starting Point)

**Best for:** E-commerce, streaming, apps with 50K+ users and sufficient interaction data

```
┌─────────────────────────────────────────────────────────┐
│                    DATA LAYER                            │
│  S3 (historical data) ──► Amazon Personalize            │
│  PutEvents API ─────────────► (real-time events)        │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│                  SERVING LAYER                           │
│  GetRecommendations API ◄── Campaign/Recommender        │
│  GetPersonalizedRanking API                             │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│                DELIVERY LAYER                            │
│  API Gateway → Lambda → AppSync → Client                │
│  DynamoDB (item metadata enrichment + caching)          │
└─────────────────────────────────────────────────────────┘
```

**Cost drivers:** Personalize campaign TPS floor, recommendation request volume, training cost

### Pattern 2: Hybrid GenAI + Behavioral Stack

**Best for:** Products that need both behavioral precision and generative explanation/conversational UI

```
┌────────────────────────────────────────────────────────────┐
│                    DATA LAYER                               │
│  S3/DynamoDB → Amazon Personalize (behavioral)             │
│  S3/Confluence/SharePoint → Bedrock Knowledge Bases         │
│  User profile store → DynamoDB                             │
└────────────────────────────────────────────────────────────┘
                          │
         ┌────────────────┴────────────────┐
         ▼                                 ▼
┌─────────────────┐              ┌──────────────────────┐
│Amazon Personalize│              │  Amazon Bedrock       │
│GetRecommendations│              │  Knowledge Base Query │
│(50 candidates)  │              │  (context retrieval)  │
└─────────────────┘              └──────────────────────┘
         │                                 │
         └─────────────┬───────────────────┘
                       ▼
              ┌─────────────────┐
              │  Bedrock FM     │
              │  (Claude/Nova)  │
              │  Re-ranking +   │
              │  Explanation    │
              └─────────────────┘
                       │
                       ▼
              Final ranked list
              with explanations
```

### Pattern 3: Full GenAI Stack (Cold Start / Early Stage)

**Best for:** New products with no behavioral data, high-touch B2B personalization, conversational commerce

```
User Request (natural language)
         │
         ▼
┌──────────────────────────────┐
│  Amazon Bedrock Agent        │
│  ├── Action: Query Knowledge │
│  │   Base (product catalog)  │
│  ├── Action: Query CRM       │
│  │   (customer profile)      │
│  ├── Action: Check inventory │
│  └── Memory: past sessions   │
└──────────────────────────────┘
         │
         ▼
Personalized recommendation + reasoning
```

### Pattern 4: Personalized Search

**Best for:** Sites with search as primary discovery mechanism

```
User Search Query
         │
         ▼
    OpenSearch
    (BM25 lexical results, 100 candidates)
         │
         ▼
  Amazon Personalize
  GetPersonalizedRanking
  (re-order by user preference)
         │
         ▼
    Personalized search results
```

### Pattern 5: Batch Marketing Personalization

**Best for:** Email campaigns, push notifications, weekly digests

```
Scheduled EventBridge
         │
         ▼
    Personalize Batch Job
    (segment users by Item-Affinity)
         │
         ▼
    S3 (user segments)
         │
         ▼
    Lambda → Bedrock
    (generate personalized copy per segment)
         │
         ▼
    SES / Pinpoint
    (deliver personalized messages)
```

---

## 5. Pricing Summary — Concrete Numbers

### Amazon Personalize

| Dimension | Cost |
|-----------|------|
| Data ingestion | $0.05/GB |
| Training | $0.002/1,000 interactions |
| Real-time recommendations | $0.15/1,000 requests |
| Batch recommendations | $0.15/1,000 requests |
| Minimum campaign cost | ~$388/month per provisioned TPS (24/7) |

### Amazon Bedrock (us-east-1)

| Model | Input (per 1M tokens) | Output (per 1M tokens) |
|-------|----------------------|----------------------|
| Claude 3.5 Sonnet | $3.00 | $15.00 |
| Claude 3 Haiku | $0.25 | $1.25 |
| Claude 3 Opus | $15.00 | $75.00 |
| Llama 2 Chat 13B | $0.75 | $1.00 |
| Google Gemma 3 4B | $0.04–0.06 | varies |
| Titan Embeddings V2 | ~$0.02/1M | — |

Batch inference: 50% discount vs on-demand

### Amazon Kendra

| Edition | Monthly Cost (base index) |
|---------|--------------------------|
| GenAI Enterprise | ~$230/month |
| Basic Enterprise | ~$1,008/month |
| Developer | ~$810/month |

### Amazon OpenSearch Serverless

- Minimum: 2 OCUs = ~$345/month (at $0.24/OCU-hour × 2 × 720 hrs)
- Vector search collections: separate OCU pool, same rates
- GPU-accelerated OCUs available for vector indexing

### AWS AppSync

| Operation | Cost |
|-----------|------|
| Queries/mutations | $4.00/million |
| Real-time updates | $2.00/million |
| Connection time | $0.08/million minutes |

### AWS Lambda

- $0.20/million requests (after free tier)
- Duration: based on memory allocation × time (varies)
- Graviton2: 20% cheaper than x86

---

## 6. Constraints and Gotchas — What to Watch

### 6.1 Cold Start — The Most Common Problem

**Severity:** High. Nearly every customer hits this.

| Scenario | Problem | Mitigation |
|----------|---------|------------|
| New user | No history → popularity fallback | Collect one interaction immediately via PutEvents; use exploration in User-Personalization-v2 |
| New item | Not recommended initially | Set exploration_weight high; use Semantic-Similarity for content-cold-start; always populate CREATION_TIMESTAMP |
| New product/launch | No catalog interactions | Use Bedrock/GenAI for first weeks; transition to Personalize once data accumulates |
| < 1,000 interactions | Cannot train Personalize at all | Use Bedrock, Kendra, or Popularity-Count in early stage |

### 6.2 Data Minimums

- 1,000 interactions: absolute minimum to create a solution
- 50,000+ interactions: where quality recommendations emerge
- If your customer has < 50K interactions, set expectations carefully

### 6.3 Campaign TPS Floor Cost Trap

- Every active campaign bills for minProvisionedTPS **continuously**
- 1 TPS × $0.15/1000 × 3,600 s/hr × 24 hr × 30 days ≈ **$388/month floor per TPS**
- For a product with variable traffic (e.g., bursts only during business hours), this is significant waste
- **Mitigation:** Use pre-computed recommendations cached in DynamoDB; call Personalize on cache miss only; or delete campaigns during off-hours (not suitable for all use cases)

### 6.4 Auto-scaling Delay

- When traffic exceeds minProvisionedTPS, Personalize scales up — but there is a **latency delay during scale-up that can drop transactions**
- Mitigation: Set minProvisionedTPS to your expected P95 load, not the minimum

### 6.5 Schema Constraints

- ITEM_ID and USER_ID max length: 256 characters
- Categorical values max: 1,000 characters
- Text fields: only 1 textual field per items dataset
- Max metadata fields: 100 per items dataset, 25 per users dataset
- These constraints are largely invisible until a customer tries to import unusual data

### 6.6 Latency Tradeoffs

| Approach | P50 Latency | P99 Latency | Notes |
|----------|-------------|-------------|-------|
| Personalize (campaign, 1 item) | ~5–15ms | ~50ms | Increases with catalog size |
| Personalize (with metadata return) | ~20–40ms | ~100ms | 50 item limit with metadata |
| DynamoDB cache lookup | ~1–3ms | ~5ms | Best for pre-computed |
| Bedrock FM inference | ~500ms–3s | ~5s | Depends on model + tokens |
| Bedrock Knowledge Base | ~200ms–2s | ~3s | RAG retrieval + FM |
| OpenSearch k-NN | ~10–50ms | ~100ms | Depends on index size |

*Note: Personalize latency numbers are empirical from AWS case studies and not formally published SLAs.*

### 6.7 Region Availability

Not all Personalize features are available in all regions. V2 recipes and domain recommenders have wider availability than legacy recipes, but check specific regional availability for Thailand-based deployments (ap-southeast-1 Singapore is the nearest region with full feature support).

### 6.8 Bedrock Token Cost at Scale

- Personalized recommendation with explanation: ~500 input tokens + 200 output tokens per request
- At Claude 3.5 Sonnet pricing: ~$1.65 + $3.00 = ~$4.65 per 1,000 requests (vs $0.15 for Personalize)
- Bedrock FM re-ranking is **30× more expensive** than Personalize for the same request count
- Use FM re-ranking sparingly: only for high-value users, conversion-critical pages, or where explanation adds conversion value

### 6.9 Bedrock Knowledge Base Limitations

- Max chunk size and retrieval context window depends on FM
- Freshness: data must be re-synced as catalog changes (manual or scheduled sync)
- Not a substitute for real-time behavioral recommendations (Personalize)

### 6.10 Personalize vs Bedrock Decision Framework

```
Do you have behavioral interaction data (clicks, purchases)?
├── YES → Use Amazon Personalize as primary engine
│   └── Do you also need explanations or conversational UI?
│       ├── YES → Add Bedrock FM as re-ranker/explainer on top
│       └── NO → Personalize alone is sufficient
└── NO (new product, sparse data)
    └── Do you have a content catalog (products, articles)?
        ├── YES → Use Bedrock Knowledge Base + Semantic-Similarity recipe
        └── NO → Use Bedrock Agent for conversational recommendation
```

---

## 7. What to Ask the Customer

### Discovery Questions

1. **Volume:** How many users? How many items/products? How many interaction events per month?
2. **Data maturity:** Do you currently collect click/view/purchase events? Where are they stored?
3. **Use case:** E-commerce product recs? Content recs? Search personalization? Email? All of the above?
4. **Latency requirement:** Real-time on-page (< 100ms)? Or is pre-computed (batch) acceptable?
5. **Cold start exposure:** What % of traffic is anonymous/new users?
6. **Content type:** Structured items (products with attributes) or unstructured content (articles, documents)?
7. **Existing infrastructure:** OpenSearch? DynamoDB? Any existing ML platform?
8. **Explanation needed?** Do recommendations need to be explainable to users? ("Recommended because...")
9. **Regulatory constraints:** Data residency? PII handling requirements?
10. **Budget ballpark:** Are they cost-sensitive or prioritizing quality?

### Qualifying for Personalize
- > 1,000 interactions: can use Personalize (barely)
- > 50,000 interactions: Personalize makes sense
- Fits VIDEO_ON_DEMAND or ECOMMERCE domain: use domain recommenders, simplest path
- Otherwise: use custom dataset group with appropriate recipe

### Qualifying for Bedrock-First
- < 1,000 interactions (new product)
- Need natural language interface
- Content-heavy (articles, documents) rather than catalog items
- Need explainability
- High-touch B2B where reasoning matters more than scale

---

## 8. Competitive Positioning vs Non-AWS Options

| Option | AWS Equivalent | AWS Advantage |
|--------|---------------|---------------|
| Google Retail AI | Amazon Personalize | Tighter AWS ecosystem integration, SageMaker/Bedrock combo |
| Azure Personalizer | Amazon Personalize | Personalize has more recipe variety and scale |
| Recombee | Amazon Personalize | Personalize = no vendor lock-in, runs in your VPC |
| Custom Python (Surprise/implicit) | SageMaker | Managed MLOps, no infra management |
| Algolia Recommend | OpenSearch + Personalize | More control, lower per-query cost at scale |

---

*Sources: AWS documentation (docs.aws.amazon.com/personalize), AWS pricing pages, AWS ML blog, AWS What's New. Verified against live pages February 2026. Always use the AWS Pricing Calculator for final quotes.*
