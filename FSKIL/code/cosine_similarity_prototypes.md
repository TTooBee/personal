---
id: cosine_similarity_prototypes
aliases: []
tags: []
---

# cosine_similarity_prototypes

## 2025/04/17
- It's good to see the embeddings with TSNE, but it'll be better to look at the cosine similarities between
  the prototypes.
- Should extract the speech, formant, and residual prototypes of the model.
- Think about the example below.
---
> [!example]
> Inside the model, there are three types of prototypes.
> 1. Speech prototypes,
> 2. Formant prototypes,
> 3. Residual prototypes.
> Calculate the similarity of different classes' prototypes, and compare the feature of these three similarity matrices.
---
> [!fail]
> I didn't replace the base_fc of formant and residual while training the model.
> Should add replacing and updating part in `replace_base_fc` in `helper.py` and `update_fc` in `Network.py`.
---
> [!success] Fixed!
---
- `prototypes_visualization.py` in `practice` folder is the code that visualizes the cosine similarity between the prototypes.
- Think I also need to calculate the similarity between the `emb` and `emb_rec`.
> [!note]
> Look at [[Scratch#2025/04/17]]
