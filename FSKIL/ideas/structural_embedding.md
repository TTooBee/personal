---
id: structural_embedding
aliases: []
tags: []
---

# structural_embedding

## 2025-03-14

### Basic
Learn the embedding with base process. This is exactly the same with the base code.
Get the ==difference vectors==(which are the result of the subtraction of the vectors that are adjacent to each other), and put it in the Transformer layer.
The prototype embedding will model the absolute position of the keywords, and the ==structural modeling layer will model the structural differences== of the keywords.
Use these two vectors as the prototype just like in statistics pooling that use mean and the deviation to model the speaker.

[[transformer_module]]

### More ideas
What should loss be like?
1. Classification loss.
2. Regularization loss.
  - This should be the sum of the distance between the adjacent vectors. And this should be done in x_ori, which is the matrix before the Transformer layer.
