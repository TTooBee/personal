---
id: unseen_performance_drops
aliases: []
tags: []
---

# unseen_performance_drops

## 2025-03-14
- Why the hell is this happening...
- When Transformer is used, then it's good. It used to be *better* when the Transformer was not used, at least on the area of unseen performance, but now it sucks.
- There may be a code only for the Transformer layer, but I didn't make it optional or something.
- Or there could be a problem with the replacing the prototypes.
- Dropped `args.num_features_pri` to 8 from 101. Let's see what happens.
  - This actually does nothing if `vector_based_only == 1`.
- When tested with test only, performance was good. Maybe related to this?
- **The model was being tested with the `logits + logits2`**.
  - Not solved...
- Let's just try with larger epochs.
- `lgt = torch.cat([lgt, logits2.cpu()])` -> `lgt = torch.cat([lgt, final_logits.cpu()])`. Think this affected the test accuracy during the incremental session.
  - **Solved!**
