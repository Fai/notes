---
title: "Natural Language Processing"
tags: [concept, ai-ml, nlp]
created: 2026-02-26
---

History
- 1954 Bag of Words
- 1972 TF-IDF
- 1997 RNN
- 2013 Word2Vec

Q K V Concept
- Query
- Key
- Value
Softmax
- Sharpen Attention
- Scale values to a probability distribution tat adds up to
Attention(Q,K,V) = softmax(Q คูณ K Transpose หารด้วย sq.root (dimension ของ k)) คูณ V
Attention
- Self-attention: Q K V from same
- Cross Attention: Q K V from different
- Multi-headed Attention: applied multiple times in parallel with different initialized weights for learning a different representation