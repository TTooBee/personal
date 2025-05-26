---
id: form_exc_attention
aliases: []
tags: []
---

# form_exc_attention

## 2025/05/22
- 15:46:19 : The code below is the part that gets the speech embeddings with attention mechanism.
```python helper.py formant_exc_attention
def formant_exc_attention(emb_2d, emb_a_freq_2d, emb_res_2d):
    # shape of emb_2d : (batch, 256, 1, 101)
    emb_2d = emb_2d.squeeze(2).transpose(2, 1)  # (batch, 101, 256), Q
    emb_a_freq_2d = emb_a_freq_2d.squeeze(2).transpose(2, 1)  # (batch, 101, 256), K
    emb_res_2d = emb_res_2d.squeeze(2)  # (batch, 256, 101), V

    attention_logit = torch.matmul(emb_a_freq_2d, emb_res_2d)  # (batch, 101, 101)
    attention_score = F.softmax(attention_logit, dim=-1)

    final_emb = torch.matmul(attention_score, emb_2d)

    return final_emb
```
- 15:47:00 : Think I should use this in `base_train`, `test`, `replace_base_fc`, `update_fc`.
  - 16:24:54 : Added `formant_exc_attention` in all the functions written above.
