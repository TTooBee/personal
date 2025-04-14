---
id: formant_residual
aliases: []
tags: []
---

# formant_residual

## 2025-03-14
- Voice can actually be separated to **formant** and **residual**. If this could be modeled inside the embedding space, then it'll be a good way to do the manifold mixup.
- And also, when we think of how we humans discriminate different keywords, if the model can imitate how we listen to the voice, then it would make a better result. But still need to think about the reason why this is better(if the result gets better).
- Make formant model and residual model(and the original model) separately?

## 2025-03-18
- Extract LPC and residual, then save them in .pt file. [[lpc#2025-03-18]]
- ==Practice code made. This function should be the default linear prediction code.==

## 2025-04-04 
- Formant is related to the **voiced signal**, and residual is related to **unvoiced signal**.
- In unvoiced frame of the speech, the formant may contain the random information that is not able to be discriminated.
- Likewise, the residual may not contain any information in the voiced frame.
- Then it seems like I should get the model learned only with the formant and only with residual.

## 2025/04/09
- Now the code is only using speech embedding to get the classification loss.
---
> [!question]
> Why don't we add the classification loss made with formant and residual embeddings?
---
- Should make the classification loss with formant and residual embeddings.
  [[formant_residual_classification_loss]]

## 2025/04/10
- Use noise speech and **clean formant and residual** during the base session.
  - The reason of the performance drop may be related to the things written below.
    > [!Example] Candidate for performance drop in noisy environment
    > - Noise may highly influenced the residual.
    > - ...anything else..?
  - So maybe I can try not to use noise augmentation in the LPC related features.
    > [[formant_residual_no_noise#2025/04/10]]
