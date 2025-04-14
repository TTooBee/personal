---
id: formant_residual_classification_loss
aliases: []
tags: []
---

# formant_residual_classification_loss

## 2025/04/09
1. Add prototypes for residual and formant(this part actually already existed, but never being used).
---
```python Network.py __init__
        self.fc_formant = nn.Linear(
            self.num_features, self.args.num_classes, bias=False
        )
        self.fc_residual = nn.Linear(
            self.num_features, self.args.num_classes, bias=False
        )
```
---

2. Make logits for formant and residual.
---
```python helper.py base_train
        logits = F.linear(
            F.normalize(emb, p=2, dim=-1),
            F.normalize(model.module.fc.weight, p=2, dim=-1),
        )
        logits_a_freq = F.linear(
            F.normalize(emb_a_freq, p=2, dim=-1),
            F.normalize(model.module.fc_formant.weight, p=2, dim=-1),
        )
        logits_res = F.linear(
            F.normalize(emb_res, p=2, dim=-1),
            F.normalize(model.module.fc_residual.weight, p=2, dim=-1),
        )
```
---

3. Smoothing, slicing, and get each loss.
---
```python helper.py base_train
        logits = args.temperature * logits  # smoothing
        logits_a_freq = args.temperature * logits_a_freq  # smoothing
        logits_res = args.temperature * logits_res  # smoothing

        logits = logits[:, : args.base_class]
        logits_a_freq = logits_a_freq[:, : args.base_class]
        logits_res = logits_res[:, : args.base_class]

        loss = F.cross_entropy(logits, train_label)
        loss_a_freq = F.cross_entropy(logits_a_freq, train_label)
        loss_res = F.cross_entropy(logits_res, train_label)
```
---

4. Add formant and residual loss to the original loss.
---
```python helper.py
            total_loss = (
                loss
                + loss_a_freq  # classification loss based on formant
                + loss_res  # classification loss based on residual
                + args.loss_lpc * loss_lpc
                + args.loss_lpc * loss_lpc_shuffled
            )
```
---

5. Replace prototypes with formant and residual embeddings(already done).
---
```python helper.py replace_base_fc
            model.module.fc.weight.data[: args.base_class] = proto_list
            model.module.fc_residual.weight.data[: args.base_class] = proto_res_list
            model.module.fc_formant.weight.data[: args.base_class] = proto_a_freq_list
```
---
> [!caution] Don't need to do anything with `test` and things related to incremental session.
> - `final_logits` in `test`.
> - `data` in `update_fc`.
