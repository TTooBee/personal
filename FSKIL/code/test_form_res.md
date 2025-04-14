---
id: test_form_res
aliases: []
tags: []
---

# test_form_res

## 2025-04-02
- The code right now only test the model with the speech embeddings. What I want to see is what happens when the model is
  tested with the formant embeddings.
- The current code gets the logits by calculating the cosine similarity between the speech embeddings and the prototypes.
- Let's just test the model with **formant embeddings** instead of speech embeddings. 
  [[April#Experiments-20250402]]
- Getting logits in both in `base_train` and `test` should be changed.

---
```python helper.py getting logits in base_train
        logits = F.linear(
            F.normalize(emb_a_freq, p=2, dim=-1), # 'emb_a_freq' was originally emb
            F.normalize(model.module.fc.weight, p=2, dim=-1),
        )
```
---
---
```python helper.py getting logits in test
            else:
                logits, logits2 = model.module.forward_metric(a_freq_data) # 'a_freq_data' was originally data
```
---

- The model does learn how to classify the keywords by formants.
- What about testing with `emb_rec`?
- In FACT and FSKIL, they used a learnable parameters to make virtual classes' prototypes.
  [[forward_compatible_few-shot_class-incremental_learning]]
