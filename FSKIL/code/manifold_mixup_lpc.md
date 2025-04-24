---
id: manifold_mixup_lpc
aliases: []
tags: []
---

# manifold_mixup_lpc

## 2025/04/22
- There could be two ways of applying manifold mixup.
1. Instance level mixup, and make two more embedding vectors with formant and residual.
2. Prototype level mixup.
- Should do number 1 first.
- The code below is the part that gets the speech based classification loss.
```python helper.py base_train
        loss = F.cross_entropy(logits, train_label)
```
- The code below is the part that gets the speech embeddings.
```python helper.py base_train
        logits = F.linear(
            F.normalize(emb, p=2, dim=-1),
            F.normalize(model.module.fc.weight, p=2, dim=-1),
        )
```
- The code below is the part that gets the speech embedding with feature extractor.
```python base_train
        emb, emb_2d = model(data)
```
- The part that gets the formant and residual embedding.
```python helper.py base_train
            emb_res, emb_res_2d = model(res_data)
            emb_a_freq, emb_a_freq_2d = model(a_freq_data)
```
- Have to make two more new vectors with these three different kinds of embeddings.
- Lean `emb` vector to the side of the `emb_a_freq` and `emb_res` each, so that I can get two more embeddings.
  - Should set the hyper-parameter(how much I'll lean the vector).
- Right after `emb` is made, make augmented vectors.
- The code below is the calibration part, and think it'll be good to reference this code to make 
  my LPC augmentation code.
```python Network.py soft_calibration
    def soft_calibration(self, args, session):
        base_protos = self.fc.weight.data[: args.base_class].detach().cpu().data
        base_protos = F.normalize(base_protos, p=2, dim=-1)

        cur_protos = (
            self.fc.weight.data[
                args.base_class + (session - 1) * args.way : args.base_class
                + session * args.way
            ]
            .detach()
            .cpu()
            .data
        )
        cur_protos = F.normalize(cur_protos, p=2, dim=-1)

        weights = torch.mm(cur_protos, base_protos.T) * args.softmax_t
        norm_weights = torch.softmax(weights, dim=1)
        delta_protos = torch.matmul(norm_weights, base_protos)

        delta_protos = F.normalize(delta_protos, p=2, dim=-1)

        updated_protos = (
            1 - args.shift_weight
        ) * cur_protos + args.shift_weight * delta_protos

        self.fc.weight.data[
            args.base_class + (session - 1) * args.way : args.base_class
            + session * args.way
        ] = updated_protos
```
- Added the argument in `train.py`.
```python train.py
    parser.add_argument("-shift_weight_lpc", type=float, default=0.1)
```
- Normalize both vectors(speech, formant or speech, residual)
- Then `(1 - shift)*speeech + shifht*formant`.

1. Normalize the embeddings.
```python helper.py base_train
        emb_norm = emb / torch.unsqueeze(
            torch.torch.norm(emb, p=2, dim=1), dim=1
        ).expand(emb.shape)

        if args.load_from_file == 1:
            emb_a_freq_norm = emb_a_freq / torch.unsqueeze(
                torch.torch.norm(emb_a_freq, p=2, dim=1), dim=1
            ).expand(emb_a_freq.shape)
            emb_res_norm = emb_res / torch.unsqueeze(
                torch.torch.norm(emb_res, p=2, dim=1), dim=1
            ).expand(emb_res.shape)
```
2. LPC based augmentation.
```python helper.py base_train
            # LPC augmentation
            emb_aug_a_freq = (
                1 - args.shift_weight_lpc
            ) * emb_norm + args.shift_weight_lpc * emb_a_freq_norm
            emb_aug_res = (
                1 - args.shift_weight_lpc
            ) * emb_norm + args.shift_weight_lpc * emb_res_norm
```
3. Get logits from LPC augmented embeddings.
```python helper.py base_tarin
        if args.load_from_file == 1:
            logits_aug_a_freq = F.linear(
                F.normalize(emb_a_freq_norm, p=2, dim=-1),
                F.normalize(model.module.fc.weight, p=2, dim=-1),
            )
            logits_aug_res = F.linear(
                F.normalize(emb_res_norm, p=2, dim=-1),
                F.normalize(model.module.fc.weight, p=2, dim=-1),
            )
```
4. Get loss from LPC augmented embeddings.
```python helper.py base_train
            loss_aug_a_freq = F.cross_entropy(logits_aug_a_freq, train_label)
            loss_aug_res = F.cross_entropy(logits_aug_res, train_label)
```
5. Add these in total loss.
```python helper.py base_train
            total_loss = (
                loss
                + args.shift_weight_lpc * (loss_aug_a_freq + loss_aug_res)
```
> [!note] Experiment result
> [[Experiments#2025/04/22]]
