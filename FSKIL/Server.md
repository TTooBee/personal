---
id: Server
aliases: []
tags: []
---

# Server

---
> [!note]
> This vaults let you know what your GPU server was doing.
> Mostly related to [[Experiments]]
---

## 2025/04/07

---

- [x] `lpc_loss` 0, make the model learn only with **formant**, 50 epochs, `noise_prob` 0. [!fail]

```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 50\
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 0 \
```

  > [!note] Result
  > It went a bit bad.. [[logits_a_freq_error#2025/04/07]]
---

- [x] Test to check whether the code based on speech embeddings only is not wrong.

```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 3\
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 0 \
```

  > [!note] Result
  > It's good. No problem with the code with speech embedding.
---

- [x] `lpc_loss` 0, make the model learn only with **residual**, 100 epochs, `noise_prob` 0.

```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100\
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 10 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 0 \
```

> [!note] Result
> Succeeded to make it work properly. [[Experiments#2025/04/09]]
---

## 2025/04/09

- [x] `lpc_loss` 0, make the model learn only with **formant**, 100 epochs, `noise_prob` 0.

```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100\
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 10 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 0 \
```

> [!note] Result
> [[Experiments#2025/04/09]]
---

- [x] Formant and residual based classification loss added(noise_prob 0.0).
  > [!note] [[formant_residual_classification_loss#2025/04/09]]
>
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -decay 0.005 \
  -lr_base 0.0005 \
  -epochs_base 100\
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 1 \
```

> [!note] Result
> [[Experiments#2025/04/09]]
---

## 2025/04/10

- [x] Formant and residual based classification loss added(noise_prob 0.8).
  > [!note] [[formant_residual_classification_loss#2025/04/09]]
>
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -decay 0.005 \
  -lr_base 0.0005 \
  -epochs_base 100\
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.8 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 1 \
```

> [!note] Result
> [[Experiments#2025/04/10]]
---

- [x] Formant and residual based classification loss added(noise_prob 0.8, but no noise to formant and residual).
  > [!note] [[formant_residual_classification_loss#2025/04/09]]
>
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -decay 0.005 \
  -lr_base 0.0005 \
  -epochs_base 100\
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.8 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 1 \
```

> [!note] Result
> [[Experiments#2025/04/10]]
---

## 2025/04/11

- [x] Formant and residual based classification loss added(noise_prob 0.0).
  `lpc_loss_shuffled` 0.
  > [[Experiments#2025/04/11]]
>
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -decay 0.005 \
  -lr_base 0.0005 \
  -epochs_base 100\
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 1 \
```

> [!note] Result
> [[Experiments#2025/04/11]]
---

- [x] `loss_lpc` 0, decreased decreased number of base classes.
  > [!note]
  > Code : [[adjust_base_class#2025/04/11]]
  > Idea : [[fewer_base_classes#2025/04/11]]
>
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -decay 0.005 \
  -lr_base 0.0005 \
  -epochs_base 100\
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 0 \
```

> [!note] Result
> [!error]  [[two_way_error#2025/04/12]]
---

## 20250412

- [-] Extract LPC related features with LPC order 20.
  > [!note]
  > Code : [[extract_lpc_data#2025/04/11]]
>
```sh python full command
practice/data.py -noise 1 -noise_prob 0.0
```
---

## 2025/04/13
- [x] `loss_lpc` 1, LPC order 20, `loss_lpc_shuffled` 0.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -decay 0.005 \
  -lr_base 0.0005 \
  -epochs_base 100\
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 1
```
> [!note] Result
> [[Experiments#2025/04/13]]
---

## 2025/04/15
- [x] `emb_rec_similarity` set to "dot".
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -decay 0.005 \
  -lr_base 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 1 \
  -loss_lpc_shuffled 0 \
  -emb_rec_similarity "dot"

python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -decay 0.005 \
  -lr_base 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 1 \
  -loss_lpc_shuffled 1 \
  -emb_rec_similarity "dot"
```
> [!note] Result
> [[Experiments#2025/04/15]]
---

## 2025/04/16
- [x] `emb_rec_similarity` set to "dot".
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 1 \
  -loss_lpc_shuffled 0 \
  -emb_rec_similarity "dot"

python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 1 \
  -loss_lpc_shuffled 1 \
  -emb_rec_similarity "dot"
```
> [!note] Result
> [[Experiments#2025/04/16]]
> Stopped the second training.
---
## 2025/04/17
- [x] lowered `loss_a_freq` and `loss_res`.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -emb_rec_similarity "dot"
```
> [!note] Result
> [[Experiments#2025/04/17]]
---
## 2025/04/18
- [x] `loss_lpc` and `loss_lpc_shuffled` set to 1, `loss_a_freq` and `loss_res` set to 0.5.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "Transformer" \
  -load_from_file 1 \
  -loss_lpc 1 \
  -loss_lpc_shuffled 1 \
  -emb_rec_similarity "dot"
```
> [!note] Result
> [[Experiments#2025/04/18]]
---
## 2025/04/19
- [x] Same setting with [[#2025/04/18]], but with `sequential_layer` 0.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 1 \
  -loss_lpc_shuffled 1 \
  -loss_a_freq_cls 0.5 \
  -loss_res_cls 0.5 \
  -emb_rec_similarity "dot"
```
> [!note] Result
> [[Experiments#2025/04/19]]
---
- [x] Same with the setting right above with extracting 2d data related to LPC.
> [!note]
> The result should be exactly the same with the experiment above. This is just for 2d data.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 1 \
  -loss_lpc_shuffled 1 \
  -loss_a_freq_cls 0.5 \
  -loss_res_cls 0.5 \
  -emb_rec_similarity "dot"
```
> [!note] Result
> [[Experiments#2025/04/19]]
---
- [x] Same with the setting right above with 2d embeddings problem fixed.
> [!note]
> The result should be exactly the same with the experiment above. This is just for 2d data.
> Code : [[2d_embeddings_with_lpc_code#2025/04/20]]
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 1 \
  -loss_lpc_shuffled 1 \
  -loss_a_freq_cls 0.5 \
  -loss_res_cls 0.5 \
  -emb_rec_similarity "dot"
```
> [!note] Result
> [[Experiments#2025/04/20]]
---
## 2025/04/21
- [x] Same setting with the last experiment in [[2025/04/19]], with `loss_lpc` and `loss_lpc_shuffled`
  set to 5.
> [!note]
> Code : [[2d_embeddings_with_lpc_code#2025/04/20]]
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 5 \
  -loss_lpc_shuffled 5 \
  -loss_a_freq_cls 0.5 \
  -loss_res_cls 0.5 \
  -emb_rec_similarity "dot"
```
> [!note] Result
> [[Experiments#2025/04/21]]
---
- [x] Same setting with the last experiment in [[2025/04/19]], with `lr_base` set to 0.002.
> [!note]
> The result should be exactly the same with the experiment above. This is just for 2d data.
> Code : [[2d_embeddings_with_lpc_code#2025/04/20]]
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.002 \
  -decay 0.0002 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 5 \
  -loss_lpc_shuffled 5 \
  -loss_a_freq_cls 0.5 \
  -loss_res_cls 0.5 \
  -emb_rec_similarity "dot"
```
> [!note] Result
> [[Experiments#2025/04/21]]
---
- [x] Same setting with the experiment above, set `loss_lpc` and `loss_lpc_shuffled` to 0.
> [!note]
> Set `loss_lpc` and `loss_lpc_shuffled` to 0.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 0.5 \
  -loss_res_cls 0.5 \
  -emb_rec_similarity "dot"
```
> [!note] Result
> [[Experiments#2025/04/21]]
---
## 2025/04/22
- [x] Same setting with the last experiment in [[#2025/04/21]] with LPC augmentation.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 1 \
  -loss_lpc_shuffled 1 \
  -loss_a_freq_cls 0.5 \
  -loss_res_cls 0.5 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> Stopped.
---
- [x] LPC augmentation with different `shift_weight_lpc`.
> [!warning]
> `loss_lpc` and `loss_lpc_shuffled` was set to 1.
> Should change this to 0.
---
> [!note] Result
> Bad..
---
## 2025/04/23
- [x] Residual augmentation.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 0.5 \
  -loss_res_cls 0.5 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/04/23]]
---
- [ ] Only with formant and residual classification loss.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 0 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 0.5 \
  -loss_res_cls 0.5 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -model_dir "/home/sehyun/Projects/FSKIL/checkpoint/gsc2/trans/ft_cos-avg_cos-data_init-start_0/0423-22-06-52-667-Epo_100-Bs_128-sgd-Lr_0.005-decay0.0005-Mom_0.9-Max_100-NormF-T_16.00/session0_max_acc.pth"
```
> [!note] Result
> The model was initially trained with 100 epochs but stopped at 88th epoch.
> [[Experiments#2025/04/23]]
---
- [x] Only with formant and residual classification loss.
  1. Formant augmentation and residual augmentation both applied.
  2. Residual augmentation only.
  3. Formant augmentation only.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 0 \
  -loss_res_cls 0 \
  -loss_aug_a_freq 1 \
  -loss_aug_res 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"

python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 0 \
  -loss_res_cls 0 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"

python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 0 \
  -loss_res_cls 0 \
  -loss_aug_a_freq 1 \
  -loss_aug_res 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/04/24]]
> [!fail]  All the experiments of LPC augmentation is done with wrong code,
>   [[code_refactoring#2025/04/24]].
---
## 2025/04/24
- [x] Only with residual classification loss.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 1 \
  -loss_lpc_shuffled 1 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/04/30]]
> [!Fail]  Stopped?
---
## 2025/04/30
- [x] Base code with the new code (50 epochs).
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \ -loss_a_freq_cls 0 \
  -loss_res_cls 0 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/04/30]]
---
## 2025/05/01
- [x] Base code with the new code (100 epochs).
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 0 \
  -loss_res_cls 0 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/01]]
___
- [ ] Set LPC classification loss(`loss_a_freq_cls`, `loss_res_cls`) to 1.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 0 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -model_dir "/home/sehyun/Projects/FSKIL/checkpoint/gsc2/trans/ft_cos-avg_cos-data_init-start_0/0501-17-34-18-445-Epo_100-Bs_128-sgd-Lr_0.005-decay0.0005-Mom_0.9-Max_100-NormF-T_16.00/session0_max_acc.pth"
```
> [!note] Result
> [[Experiments#2025/05/01]]
> Training with 100 epochs went to nan loss. It was stopped and tested with the model that made the best result.
---
- [ ] Experiment with `comp` code(`noise_prob` 0.0).
```sh full config
python train.py comp \
  -project comp \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 0 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/01]]
---
## 2025/05/02
- [x] Experiment with `comp` code(`noise_prob` 0.8).
```sh full config
python train.py comp \
  -project comp \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.8 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 0 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/02]]
---
## 2025/05/03
- [x] Set `loss_aug_a_freq` and `loss_aug_res` to 1.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.8 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 0 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 1 \
  -loss_aug_res 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/04]]
> `load_from_file` option was set to 1. Why..?
---
## 2025/05/04
- [x] Same setting with the experiment above, `load_from_file` set to 1.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.8 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 0 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 1 \
  -loss_aug_res 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/05]]
---
## 2025/05/07
- [x] Experiment with [[manifold_mixup_lpc#2025/05/07]], only 30 epochs using pre-trained model.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 1 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 30 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -model_dir "/home/sehyun/Projects/FSKIL/checkpoint/gsc2/trans/ft_cos-avg_cos-data_init-start_0/0501-19-55-20-175-Epo_0-Bs_128-sgd-Lr_0.005-decay0.0005-Mom_0.9-Max_100-NormF-T_16.00/session5_max_acc.pth"
```
> [!note] Result
> [[Experiments#2025/05/07]]
---
## 2025/05/08
- [ ] Experiments with [[manifold_mixup_lpc#2025/05/07]], 10 epochs without using pre-trained model.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 10 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/08]]
---
- [x] Experiments with [[manifold_mixup_lpc#2025/05/07]], 30 epochs without using pre-trained model.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 30 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/08]]
---
- [x] Experiments with [[manifold_mixup_lpc#2025/05/07]], 100 epochs without using pre-trained model.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/08]]
---
- [x] Experiments with [[manifold_mixup_lpc#2025/05/07]] but applies the residual replacement
  at the beginning of the base session, 10 epochs without using pre-trained model.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 10 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/08]]
---
- [x] Same setting with the experiment above, 30 epochs.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 30 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/08]]
---
- [x] Same setting with the experiment above, 100 epochs.
```sh full config
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 30 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/08]]
---
- [x] Experiment with number 2 in [[May#Code-2025/05/09]]([[manifold_mixup_lpc#2025/05/09]]), 10 epochs
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 10 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -use_feature_generator_for_res_replacement 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
---
- [x] Experiment with number 2 in [[May#Code-2025/05/09]]([[manifold_mixup_lpc#2025/05/09]]), 100 epochs
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -use_feature_generator_for_res_replacement 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/09]]
---
- [x] Experiment with number 3 in [[#Code-2025/05/09]], 10 epochs.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 10 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -use_feature_generator_for_res_replacement 1 \
  -use_dummy_classifier_for_speech 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/09]]
---
- [x] Experiment with number 3 in [[#Code-2025/05/09]], 10 epochs, without feature generator.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 10 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/09]]
---
- [x] Experiment with number 3 in [[#Code-2025/05/09]], 100 epochs.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/09]]
---
## 2025/05/10
- [x] Experiment with number 3 in [[#Code-2025/05/09]], 30 epochs.
```sh gsc2.sh
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 30 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/10]]
---
- [x] Experiment with [[manifold_mixup_lpc#structure_vector-2025/05/12]], 10 epochs.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 10 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos"
```
> [!note] Result
> [[Experiments#2025/05/12]]
---
- [x] Experiment with [[manifold_mixup_lpc#structure_vector-2025/05/12]], 50 epochs.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 50 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 10
```
> [!note] Result
> [[Experiments#2025/05/12]]
---
## 2025/05/13
- [x] Experiment with [[manifold_mixup_lpc#Single dummy classifier-2025/05/13]], 10 epochs,
  match "three" with "eight".
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 10 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0
```
> [!note] Result
> [[Experiments#2025/05/13]]
---
- [x] Experiment with [[manifold_mixup_lpc#Single dummy classifier-2025/05/13]], 10 epochs,
  match "three" with "two".
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 10 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0
```
> [!note] Result
> [[Experiments#2025/05/13]]
---
- [x] Experiment with [[manifold_mixup_lpc#Single dummy classifier-2025/05/13]], 10 epochs,
  match "three" with "two", use lamdas.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 10 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0
```
> [!note] Result
> [[Experiments#2025/05/13]]
---
- [x] Experiment with [[manifold_mixup_lpc#Single dummy classifier-2025/05/13]], 10 epochs,
  match "three" with "two", using lamdas and new structure vector.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 10 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0
```
> [!note] Result
> [[Experiments#2025/05/13]]
> NaN occurs.
---
- [-] Experiment with FACT, set `loss_res_replace` and `loss_res_replace_fact` to 1.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -loss_res_replace_fact 1 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0
```
> [!note] Result
> [[Experiments#2025/05/13]]
---
- [x] Experiment with FACT, set `loss_res_replace` 0, `loss_res_replace_fact` to 1.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 0 \
  -loss_res_replace_fact 1 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0
```
> [!note] Result
> [[Experiments#2025/05/13]]
---
- [x] Experiment with FACT, set `loss_res_replace` 0, `loss_res_replace_fact` to 1,
  `epoch_guideline` 10.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -loss_res_replace_fact 1 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 1 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 10
```
> [!note] Result
> [[Experiments#2025/05/13]]
---
## 2025/05/14
- [x] Experiment with [[manifold_mixup_lpc#structure_vector-2025/05/12]], 100 epochs.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -loss_res_replace_fact 0 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0
```
> [!note] Result
> [[Experiments#2025/05/14]]
---
- [x] Fine tune the model below(LPC classification loss based model), 1 epoch.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 1 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -loss_res_replace_fact 0 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0 \
  -model_dir "/home/sehyun/Projects/FSKIL/checkpoint/gsc2/trans/ft_cos-avg_cos-data_init-start_0/0501-19-55-20-175-Epo_0-Bs_128-sgd-Lr_0.005-decay0.0005-Mom_0.9-Max_100-NormF-T_16.00/session5_max_acc.pth"
```
> [!note] Result
> [[Experiments#2025/05/14]]
---
- [ ] Use `get_least_similar_emb_idx`, set `loss_res_replace` to 1, 10 epochs.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 10 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -loss_res_replace_fact 0 \
  -loss_sim_speech_form 0 \
  -loss_sim_speech_res 0 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0 \
  -epoch_freeze_classifier 1000 \
  -final_logit_metric "speech"
```
> [!note] Result
> [[Experiments#2025/05/14]]
---
- [x] Experiment with [[manifold_mixup_lpc#Least and most similar embeddings-2025/05/14]], 10 epochs.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 10 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -loss_res_replace_most_sim 1 \
  -loss_res_replace_a_freq_most_sim 1 \
  -loss_res_replace_fact 0 \
  -loss_sim_speech_form 0 \
  -loss_sim_speech_res 0 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 1 \
  -epoch_freeze_classifier 1000 \
  -final_logit_metric "speech"
```
> [!note] Result
> [[Experiments#2025/05/14]]
---
- [x] Experiment with [[manifold_mixup_lpc#Least and most similar embeddings-2025/05/14]], 100 epochs.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -loss_res_replace_most_sim 1 \
  -loss_res_replace_a_freq_most_sim 1 \
  -loss_res_replace_fact 0 \
  -loss_sim_speech_form 0 \
  -loss_sim_speech_res 0 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 10 \
  -epoch_freeze_classifier 1000 \
  -final_logit_metric "speech"
```
> [!note] Result
> [[Experiments#2025/05/14]]
---
## 2025/05/15
- [x] Experiment with [[manifold_mixup_lpc#Least and most similar embeddings-2025/05/14]], with 
  `emb_replace_mode` set to `sturct`, 100 epochs.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -loss_res_replace_most_sim 1 \
  -loss_res_replace_a_freq_most_sim 1 \
  -loss_res_replace_fact 0 \
  -loss_sim_speech_form 0 \
  -loss_sim_speech_res 0 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0 \
  -epoch_freeze_classifier 1000 \
  -final_logit_metric "speech" \
  -emb_replace_mode "add"
```
> [!note] Result
> [[Experiments#2025/05/15]]
> Stopped.
---
- [x] Experiment with [[manifold_mixup_lpc#Least and most similar embeddings-2025/05/14]],
  without using least similar residual embeddings for augmentation, 100 epochs.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -loss_res_replace_most_sim 1 \
  -loss_res_replace_a_freq_most_sim 1 \
  -loss_res_replace_fact 0 \
  -loss_sim_speech_form 0 \
  -loss_sim_speech_res 0 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0 \
  -epoch_freeze_classifier 1000 \
  -final_logit_metric "speech" \
  -emb_replace_mode "add"
```
> [!note] Result
> [[Experiments#2025/05/15]]
---
- [x] LPC classification loss with current code, 100 epochs.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 100 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 0 \
  -loss_res_replace_most_sim 0 \
  -loss_res_replace_a_freq_most_sim 0 \
  -loss_res_replace_fact 0 \
  -loss_sim_speech_form 0 \
  -loss_sim_speech_res 0 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0 \
  -epoch_freeze_classifier 1000 \
  -final_logit_metric "speech" \
  -emb_replace_mode "add"
```
> [!note] Result
> [[Experiments#2025/05/15]]
---
## 2025/05/19
- [x] Experiment with [[insert_incremental_session#Matrix shape mixmatch-2025/05/19]],
  only use LPC classification loss, 30 epochs.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 30 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 0 \
  -loss_res_replace_most_sim 0 \
  -loss_res_replace_a_freq_most_sim 0 \
  -loss_res_replace_fact 0 \
  -loss_sim_speech_form 0 \
  -loss_sim_speech_res 0 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0 \
  -epoch_freeze_classifier 1000 \
  -final_logit_metric "speech" \
  -emb_replace_mode "add"
```
> [!note] Result
> [[Experiment#2025/05/19]]
---
- [x] Experiment with [[insert_incremental_session#Matrix shape mixmatch-2025/05/19]], base, 30 epochs.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 30 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 0 \
  -loss_res_cls 0 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 0 \
  -loss_res_replace_most_sim 0 \
  -loss_res_replace_a_freq_most_sim 0 \
  -loss_res_replace_fact 0 \
  -loss_sim_speech_form 0 \
  -loss_sim_speech_res 0 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0 \
  -epoch_freeze_classifier 1000 \
  -final_logit_metric "speech" \
  -emb_replace_mode "add"
```
> [!note] Result
> [[Experiment#2025/05/19]]
---
- [-] Experiment with [[insert_incremental_session#Matrix shape mixmatch-2025/05/19]],
  residual replace, 30 epochs.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 30 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 1 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 0 \
  -loss_res_cls 0 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -loss_res_replace_most_sim 0 \
  -loss_res_replace_a_freq_most_sim 0 \
  -loss_res_replace_fact 0 \
  -loss_sim_speech_form 0 \
  -loss_sim_speech_res 0 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0 \
  -epoch_freeze_classifier 1000 \
  -final_logit_metric "speech" \
  -emb_replace_mode "add"
```
> [!note] Result
> [[Experiment#2025/05/19]]
---
## 2025/05/22
- [x] 16:37:15 : Experiment with [[form_exc_attention#2025/05/22]]
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 10 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 0 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 0 \
  -loss_res_cls 0 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 0 \
  -loss_res_replace_most_sim 0 \
  -loss_res_replace_a_freq_most_sim 0 \
  -loss_res_replace_fact 0 \
  -loss_sim_speech_form 0 \
  -loss_sim_speech_res 0 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0 \
  -epoch_freeze_classifier 1000 \
  -final_logit_metric "speech" \
  -emb_replace_mode "add"
```
> [!note] Result
> [[Experiment#2025/05/22]]
---
- [x] 22:15:25 : Turn all the attention off, and experiment with residual replace, 10 epochs.
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 10 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 0 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 1 \
  -loss_res_cls 1 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 1 \
  -loss_res_replace_most_sim 0 \
  -loss_res_replace_a_freq_most_sim 0 \
  -loss_res_replace_fact 0 \
  -loss_sim_speech_form 0 \
  -loss_sim_speech_res 0 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0 \
  -epoch_freeze_classifier 1000 \
  -final_logit_metric "speech" \
  -emb_replace_mode "add"
```
> [!note] Result
> [[Experiments#2025/05/22]]
---
- [x] 23:33:43 : Experiment with [[manifold_mixup_lpc#Spectrum boundary detection-2025/05/24]].
```sh gsc2.sh
python train.py trans \
  -project trans \
  -dataset gsc2 \
  -dataroot ./data/datasets \
  -base_mode 'ft_cos' \
  -new_mode 'avg_cos' \
  -lr_base 0.005 \
  -decay 0.0005 \
  -epochs_base 30 \
  -batch_size_base 128 \
  -schedule Cosine \
  -tmax 100 \
  -gpu '0' \
  -temperature 16 \
  -spec_aug_mode 'spec' \
  -noise_prob 0.0 \
  -matrix_loss_metric 'attention' \
  -num_features_pri 101 \
  -vector_based_only 1 \
  -sequential_layer "None" \
  -load_from_file 0 \
  -loss_lpc 0 \
  -loss_lpc_shuffled 0 \
  -loss_a_freq_cls 0 \
  -loss_res_cls 0 \
  -loss_aug_a_freq 0 \
  -loss_aug_res 0 \
  -loss_res_replace 0 \
  -loss_res_replace_most_sim 0 \
  -loss_res_replace_a_freq_most_sim 0 \
  -loss_res_replace_fact 0 \
  -loss_sim_speech_form 0 \
  -loss_sim_speech_res 0 \
  -use_feature_generator_for_res_replacement 0 \
  -use_dummy_classifier_for_speech 0 \
  -shift_weight_lpc 0.1 \
  -emb_rec_similarity "cos" \
  -epoch_guideline 0 \
  -epoch_freeze_classifier 1000 \
  -final_logit_metric "speech" \
  -emb_replace_mode "add"
```
> [!note] Result
> [[Experiments#2025/05/25]]
