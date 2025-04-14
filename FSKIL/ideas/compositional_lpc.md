---
id: compositional_lpc
aliases: []
tags: []
---

# compositional_lpc

## 2025-03-31
- Compositional FSKIL says that it is good to calibrate the prototypes with compositional information.
- This idea is about making compositional information with formant and residual.
- In FSKIL, the vector that are made with compositional information is learned by the sample that SpecAugment is applied.
- Maybe this can be made with formant embeddings and residual embeddings from different classes.
> [!NOTE] Example 
> 20 base classes. New vector is made with 1st class' formant embedding and 2nd class' residual embedding.
> Always, the formant  is the criterion. So this new class is nominated to 21th class.
- There is a part of feature generator in FSKIL. Would I need this?
- Maybe should read paper about Manifold Mixup.
[[Forward_Compatible_Few-Shot_Class-Incremental_Learning]]
- To do this, prototype for formant and residual should be saved(the current code only saves the prototype of the speech spectrum).
