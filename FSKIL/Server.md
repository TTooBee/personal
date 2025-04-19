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
