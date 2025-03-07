---
id: Scratch
aliases: []
tags: []
---

# Scratch

- Loss 2d modified. Maybe the base training accuracy does not perform that well is because of the different learning rate between vector based processes and the matrix based processes. [[experiments]]
- All I need to think about where I should apply the 2d base learning process is the **test** procedure and the **loss** procedure
- Learning rate 0.005, loss_2d 0.2, ==test with vector embeddings==, ==loss with both vector and matrix embeddings==.
- Should compare this result with the base code.
- Cosine similarity loss may be better at some parts, and CKA may be better at something else. These two do not seem like one is just better than the other one.
- So don't just try to add the loss and make the model do everything, and see the result and analyse it. Maybe CKA can me used to calibration or froward compatible methods or any other things that are not just letting the model do everything.
- Then I need to make the code to analyse the result...
- If I try to analyse two models, vector based(base code) and the matrix based(primitive code) models, how should I analyse it..?
- Think I don't have anything about dummy classifier.
