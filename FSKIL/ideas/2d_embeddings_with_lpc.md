---
id: 2d_embeddings_with_lpc
aliases: []
tags: []
---

# 2d_embeddings_with_lpc

## 2025/04/19
- Now the code compares the embeddings(emb to emb or emb to prototype) with the embedding after the pooling layer.
- And this does have an effect of formant and residual.
- But could this have an effect to the 2d embeddings?
- Think about the keyword "tree". There are sequential embeddings, and at the initial part, the size of
  the residual embeddings should be larger than the size of the formant embeddings, since 't' is an unvoiced sound.
- Can I look at the 2d embeddings before the pooling layer?
  - Now it's only able to see the `fc_map` which is the speech embeddings before the pooling layer.
  - Not able to see the sequential embeddings of formant and residual.
  > [!note]
  > Code to make this possible : [[2d_embeddings_with_lpc_code#2025/04/19]]

## 2025/04/20
- First I need to look at the 2d embeddings of speech, formant, and residual of the keyword "tree" and "three".
> [!warning]
> `fc_map` and `fc_map_residual` seems exactly the same now.
> The problem is not from `prototype_visualization.py`. Think it's from the training code.
> Fix : [[2d_embeddings_with_lpc_code#2025/04/20]]
- [!success] Solved!
