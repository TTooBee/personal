---
id: code_refactoring
aliases: []
tags: []
---

# code_refactoring 

## 2025/04/24

### Overview
- After this work is done, the branches of the project should be like:
> [!note] Branches
> 1. main
> 2. feature/base [[#feature/base]]
>   - This does not use prepared pt files.
>   - All the LPC related branches will me merged to this branch.
> 3. feature/2d-embeddings [[#feature/2d-embeddings]]
> 4. feature/lpc [[#feature/lpc]]
>   - `args.load_from_file` is always 1 in this branch, so just remove all the parts from audio files.
> 5. feature/old
- `main` branch will always be feature/lpc.

### feature/lpc
- Remove all the parts that use the data that are not from pt files.
  - [x] Parts in `helper.py`.
  - [x] Parts in `Network.py`.
- Remove all the parts that uses the 2d embeddings.
  - [x] parts in `helper.py`.
  - [x] parts in `Network.py`.
---
> [!warning]
> There was a serious problem in the code.
---
```python helper.py
        logits_aug_a_freq = F.linear(
            F.normalize(emb_a_freq_norm, p=2, dim=-1), # wrong part
            F.normalize(model.module.fc.weight, p=2, dim=-1),
        )
        logits_aug_res = F.linear(
            F.normalize(emb_res_norm, p=2, dim=-1), # wrong part
            F.normalize(model.module.fc.weight, p=2, dim=-1),
        )
```
---
> [!important]
> - `emb_a_freq_norm` and `emb_res_norm` have to be changed to `emb_aug_a_freq` and `emb_aug_res`.
> - Update todo : [[April#Expriments-2025/04/24]]
---

### feature/base
- The code in this branch does not use pt files.
- Branch `feature/base` is from the branch `feature/lpc`, and should remove all the parts related to LPC.
  - [-] Parts in `helper.py`.
  - [ ] Parts in `Network.py`.
  > [!important]
  > Do not do anything with dataloader. Also, dataloader returns different numbers of data(just data
  > and label or data and LPC related data), So modify all the parts that dataloader returns more than 
  > 2 variables.

### feature/2d-embeddings
- Think I can replace an old branch to this one.
