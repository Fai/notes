---
title: "Natural Language Processing"
tags: [concept, ai-ml, nlp]
created: 2026-02-26
---

# Natural Language Processing

## History

| Year | Milestone |
|------|-----------|
| 1954 | Bag of Words |
| 1972 | TF-IDF |
| 1997 | RNN |
| 2013 | Word2Vec |

## Q K V Concept

- **Query** — what we're looking for
- **Key** — what each token offers
- **Value** — the actual content to retrieve

**Attention formula:**

```
Attention(Q, K, V) = softmax(Q · Kᵀ / √dₖ) · V
```

**Softmax** sharpens attention by scaling values to a probability distribution that adds up to 1.

## Attention Types

| Type | Description |
|------|-------------|
| Self-Attention | Q, K, V all come from the same sequence |
| Cross-Attention | Q, K, V come from different sequences |
| Multi-Head Attention | Applied multiple times in parallel with different initialized weights to learn different representations |
