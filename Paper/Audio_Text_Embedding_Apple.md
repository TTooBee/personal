---
id: Audio_Text_Embedding_Apple
aliases: []
tags: []
---

# Audio_Text_Embedding_Apple

## 2025-03-28

### Keywords
- Grapheme to Phoneme.
- Phoneme to Vector.
- Dynamic Sequence Partitioning.
- Confusable keyword generation

### Main Method
- Match the text embeddings and the audio embeddings in the latent space in the phoneme level.
- Tiny Conformer is used and CTC loss is applied to generated the sequential embeddings matched to the keyword phoneme.
- G2P model and P2V model is used to generate phoneme level vector.
- Calculate cosine similarity between audio embeddings and text embeddings, and apply DSP to mask the overlapping vectors.
- GRU -> FC.

## 2025/04/10
- Think I should know more about this paper.

