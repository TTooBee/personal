---
id: Scratch
aliases: []
tags: []
---

# Scratch

## 2025-03-08

- Loss 2d modified. Maybe the base training accuracy does not perform that well is because of the different learning rate between vector based processes and the matrix based processes. [[experiments]]
- All I need to think about where I should apply the 2d base learning process is the **test** procedure and the **loss** procedure
- Learning rate 0.005, loss_2d 0.2, ==test with vector embeddings==, ==loss with both vector and matrix embeddings==.
- Should compare this result with the base code.
- Cosine similarity loss may be better at some parts, and CKA may be better at something else. These two do not seem like one is just better than the other one.
- So don't just try to add the loss and make the model do everything, and see the result and analyse it. Maybe CKA can me used to calibration or froward compatible methods or any other things that are not just letting the model do everything.
- Then I need to make the code to analyse the result...
- If I try to analyse two models, vector based(base code) and the matrix based(primitive code) models, how should I analyse it..?
- Think I don't have anything about dummy classifier.

## 2025-03-09

- What should I do now... Now I have the result of the 2d based model and the vector based model, show I think I can compare these two, and see the actual distribution of  the model.
- Think I should separate the mode between 2d-based and the original.
- Now we need to visualize the distribution_dict. But isn't this actually is the exact same thing as the confusion matrix? Yes it is. Don't need to do this. Just see the confusion matrix.
- Seq length can be modified by in `bcresnet.py` file.
- When gram matrix is got from a matrix, **sequence of the matrix does not matter anymore**. Sequence of the vectors in the matrix can not differentiate the matrix. But it the keyword spotting task, sequence do matter because this data is actually temporal data.
- So the thing is, CKA loss may work on image data(since there is no sequence in the images), but won't work in the keyword spotting task.
- Vectors in the matrix should be matched 1 on 1 to consider the sequence of the data.
- One more thing to think when 1 on 1 matching is being done(related to CTC?). People my the same words in various different ways, so should I think about the CTC related thing to match this? Or would the model do everything?
- Then isn't this actually the same from the speech recognition task?
