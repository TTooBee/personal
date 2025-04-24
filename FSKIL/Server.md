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
- [ ] Only with formant and residual classification loss.
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
