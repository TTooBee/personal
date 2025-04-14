---
id: Forward_Compatible_Few-Shot_Class-Incremental_Learning
aliases: []
tags: []
---

## 2025/03/31

### Keywords
- Manifold Mixup.
- Forward compatible learning.
- Instance : the embedding vectors in the middle of the model(before classification).
- Prototype : final embedding used as the criterion of the classification.

### Main Method
- The key thing is to predict the future class' embedding, and preemptively reserve the space for the future embeddings.
- The paper proposes two things.
  - [Prototype level Manifold Mixup](#Prototype-Level-Manifold-Mixup)
  - [Instance level Manifold Mixup](#Instance-Level-Manifold-Mixup)
- This randomly happens inside the batch.
---
> [!note]
> Should focus on the example written below.
---
> [!example]
> - Prototype level mixup with class 1 and class 2.
> - Instance level mixup with class 1 and class 2.

#### Prototype-Level-Manifold-Mixup
- Main concept is like there are **two labels** for **one sample**.
- A sample of class 1 gets in to the feature encoder, and it's compared with two prototypes: first class' prototype,
and eleventh class' prototype.

#### Instance-Level-Manifold-Mixup
---
> [!tip]
> What I did not understand is, there is no specific label for a virtual instance.
---
- Consider that a virtual instance `z` is made from a embedding made form class 1 and a embedding from class 2.
- There will be a final embedding at the end layer of the feature encoder, and there should be a closest
prototype(*whatever it is*) for this embedding.
- This is the **pseudo label** for the vector made with manifold mixup.

### Further Thoughts-20250331
- This paper applies Manifold Mixup to embeddings from two different classes.
- For speech, we can extract **formant** and **residual**, and these two different modalities can be used in Manifold Mixup.
  [[compositional_lpc#2025-03-31]]
