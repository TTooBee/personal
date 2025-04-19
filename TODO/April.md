---
id: April
aliases: []
tags: []
---

# Thing to do

## 2025-04-01

### Code-20250401

- [ ] Add negative loss. The current code only applies the positive loss.
  [[contrastive_loss#2025-04-01]]
- [ ] Think about the way to apply Manifold Mixup in this code. It's a bit more complicated than I thought initially.

## 2025-04-02

### Idea-20250402

- [ ] Separate subspace for formant and residual.
  [[subspace_formant_residual#2025-04-02]]
  
### Code-20250402

- [x] Make code that gets the embeddings. [[test_form_res#2025-04-02]]

### Experiments-20250402

- [x] Test with formant embeddings.
  [[test_form_res#2025-04-02]], [[Experiments#2025-04-02]]

## 2025-04-03

### Iterm2-20250403

- [x] Use tmux to make it pertty.

### Code-20250403

- [ ] Make code for visualization. [[embedding_visualization#2025-04-03]]

## 2025-04-04

### Study-20250404

- [ ] Search for some papers related to decomposition of the embedding or speech(or any kind of data).

---
> [!note]
> Think this is the moment to slow down the experiments and study a bit more to make my idea better
> Let's look at FACT first.
> [[Forward_Compatible_Few-Shot_Class-Incremental_Learning]]
---

### Idea-20250404

- [ ] Think about what actually formant and residual embedding mean.
  [[formant_residual#2025-04-04]]

### Experiments-20250404

- [x] Set `loss_lpc` to 1, `loss_lpc_shuffled` to 1 without noise, 50 epochs.
  [[Experiments#2025-04-04]]
- [x] Make the model only learn with the **formant** embeddings.
  [[Experiments#2025-04-04]]
- [ ] Make the model only learn with the **residual** embeddings.

---
> [!warning]
> Remove the part indicated in [[lpc#2025-04-04]].
---
> [!warning] warning related to the note
> Date style changed from 'yyyy-mm-dd' to 'yyyy/mm/dd'!
---

## 2025/04/05

### Study-2025/04/05

- [ ] [[Forward_Compatible_Few-Shot_Class-Incremental_Learning]]

### Experiments-2025/04/05

- [x] [!fail] Make the model only learn with the residual embeddings.
  [[Experiments#2025/04/05]]
- [ ] Make the model learn with LPC.
  [[Experiments#2025/04/05]]

### Code-2025/04/05

---
> [!warning]
> Think the code for model learn with formant and residual was not correct.
> The model was being tested by the speech embeddings and the prototypes were getting replaced with speech embeddings.
---

- [x] Make the code form **only formant**, **only residual**, **only LPC** as three different branches.
  [[lpc#2025/04/05]]

## 2025/04/06

### Experiments-2025/04/06

- [ ] Model only with formant. [[lpc#2025/04/05]]
- [ ] Model only with residual. [[lpc#2025/04/05]]
- [ ] Model only with LPC. [[lpc#2025/04/05]]

## 2025-04-07

### Code-2025/04/07

[!error] Fix

- [x] Fix the code only make the model learn with formantdoesn't work, when variable `final_logits` is set to `logits_a_freq`.
  [[logits_a_freq_error#2025/04/07]]
- [ ] Fix Formant only embeddings code. [[logits_a_freq_error#formant_error-2025/04/07]]
- [ ] Try make the code learn only with residual first before testing with formant.
  [[lpc#2025/04/05]]
  [[logits_a_freq_error#residual_error-2025/04/07]]

## 2025/04/08

### Paper-2025/04/08

- [x] [[Forward_Compatible_Few-Shot_Class-Incremental_Learning]]

### Code-2025/04/08

[!error] Fix

- [x] Fix the problem that the performance of the model during the incremental session gets terrible when the model is made only with residual embeddings. [[logits_a_freq_error#2025/04/08]] [!success]

### Experiments-2025/04/08

- [x] Model only with residual. [[lpc#2025/04/05]]
  [[Experiments#2025/04/09]]
- [ ] Model only with formant. [[lpc#2025/04/05]]
- [x] [!fail] Dot Similarity, base. [[lpc#2025/04/05]]

### Idea-2025/04/08

- [ ] Don't just use formant and residual embedding as contrastive learning related things,
  but also the thing with classification. [[formant_residual#2025/04/09]]

## 2025/04/09

### Experiments-2025/04/09

- [x] Model only with formant.
  > [!note]
  > Check before you run the code : [[lpc#2025/04/05]]
  > What server is doing : [[Server#2025/04/09]]
  > Result : [[Experiments#2025/04/09]]
- [-] Formant residual classification loss added(noise_prob 0.0).
  > [!note]
  > Code : [[formant_residual_classification_loss#2025/04/09]]
  > Result : [[Experiments#2025/04/09]]

### Code-2025/04/09

- [x] Make code that classification loss based on formant and residual embeddings are added.
  > [!note]
  > Modified codes : [[formant_residual_classification_loss#2025/04/09]]

### Study-2025/04/09

- [-] Covariance. [[covariance#2025/04/09]]

### Paper-2025/04/09

- [x] Search for some papers that I can look up(with Claude).
  - Downloaded all the papers in "~/Desktop/Keyword Spotting/claude recommendation".

## 2025/04/10

### Experiments-2025/04/10

- [-] Formant residual classification loss added(noise_prob 0.8).
  > [!note]
  > Code : [[formant_residual_classification_loss#2025/04/09]]
  > Result : [[Experiments#2025/04/10]]
- [-] Make a table in [[Experiments]] to make it easy to see the results from different settings
  of the code.

### NeoVim-2025/04/10

- [x] Make neovide work on server conveniently.
  > [!note]
  > Code : [[neovide_server#2025/04/10]]
- [x] Apply the code that fixes the problem with neovide window.
  > [!note]
  > Github Link : [NeoHub](https://github.com/alex35mil/NeoHub)

### Idea-2025/04/10

- [ ] [[formant_residual#2025/04/10]]
- [ ] There's a confusion between **four(class number 18)** and **forward(class number 35)**,
  and **three(class number 19)** and **tree(class number 30)**.
  > [[confusing_keywords#2025/04/11]]

### Code-2025/04/10

- [x] Use noisy speech and clean formant and clean residual.
  > [!note]
  > Idea : [[formant_residual#2025/04/10]]
  > Code : [[formant_residual_no_noise#2025/04/10]]

### Paper-2025/04/10

- [ ] [[Audio_Text_Embedding_Apple#2025/04/10]]
- [-] [[DeepSeek-R1#2025/04/10]]

## 2025/04/11

### Lab-2025/04/11

- [x] Meeting materials.

### Experiments-2025/04/11

- [x] Same setting, set `loss_lpc_shuffled` to 0.
  > [!note]
  > Code : [[Experiments#2025/04/11]]
  > Result : [[Experiments#2025/04/11]]
- [-] Train the model with fewer base classes, `loss_lpc` 0.
  > [!note]
  > Code : [[adjust_base_class#2025/04/11]]
  > Result : [[Experiments#2025/04/11]]
  > - `args.base_class` 15
  > - `args.num_classes` 35
  > - `args.way` 2
  > - `args.shot` 5
  > - `args.session` 11 (base session + 10 incremental sessions)

### Code-2025/04/11

- [ ] Get the embeddings of speech, formant, and residual.
  > [!note]
  > [[save_embeddings#2025/04/11]]

### Idea-2025/04/11

- [ ] Think about some solutions to discriminate confusing keywords.
  > [[confusing_keywords#2025/04/11]]
- [ ] Extract the LPC related features with higher order.
  > [[extract_lpc_data#2025/04/11]]
- [-] Try to decrease the number of the base classes, and increase the incremental classes.
  > [!note]
  > ~~Think this should be done prior to making something that discriminates the confusing keywords.~~
  > Code about the number of base classes : [[adjust_base_class#2025/04/11]]

## 2025/04/12

### Idea-2025/04/12

- [x] Extract the LPC related features with higher order.
  > [!note]
  > [[extract_lpc_data#2025/04/11]]
  > [[Server#2025/04/12]]

### Experiments-2025/04/12

- [x] Same setting, LPC order 20(but `loss_lpc` was set to 0).
  > [[Experiments#2025/04/12]]
- [ ] Only residual, base.
- [ ] Only formant, base.

## 2025/04/13

### Experiments-2025/04/13

- [x] Same setting, LPC order 20.
  > [[Experiments#2025/04/12]]
  > [[Server#2025/04/13]]

## 2025/04/14

### Code-2025/04/14
- [ ] See the confusion matrix.

### Study-2025/04/14

### NeoVim-2025/04/14
- [ ] Try Nord.
- [x] Split my Todo list. The file can be too large, so maybe it'll be a good idea to split it
  by weeks or a month.
  - It's been a month to use obsidian.nvim as my main note, and now it's about 500 lines,
    so I think it'll be good to split the note my months.
  - Or weeks..
  - Use Claude to do this.

## 2025/04/15

### Code-2025/04/15
- [x] Make the code work with dot similarity when comparing the speech embeddings and LPC embeddings.
  > Code : [[dot_similarity#2025/04/15]]

### Experiments-2025/04/15
- [x] Use dot similarity between `emb_rec` and `emb` instead of cosine similarity.
  > Code : [[dot_similarity#2025/04/15]]
  > Result : [[Experiments#2025/04/15]]
- [x] Same config, `lpc_loss_shuffled` 1.
  > Code : [[dot_similarity#2025/04/15]]
  > Result : [[Experiments#2025/04/15]]

### Tomorrow-2025/04/15
- Change the ==min-max== standardization with ==mean-std== standardization.
  > [!note]
  > Base code : [[dot_similarity#2025/04/15]]

## 2025/04/16

### Experiments-2025/04/16
- [ ] ~~Same setting with [[Experiments-2025/04/15]](`lpc_loss_shuffled 1`), increased lr.~~
  > Code : [[dot_similarity#2025/04/15]]
  > Result : [[Experiments#2025/04/16]]
---
> [!warning]
> `lr_based` was set to 0.0005. Experiments should be done again with `lr_base` 0.005.
---
- [x] [[Experiments-2025/04/15]] with `lr_base` 0.005.
  > Result : [[Experiments#2025/04/16]]

### Paper-2025/04/16
- [ ] FSKIL.
  > [[FSKIL#2025/04/16]]

### Code-2025/04/16
- [ ] Look at the speech, formant, residual prototypes.
  > [[confusing_keywords#2025/04/16]]

### Idea-2025/04/16
- Use a codebook for a residual?
  > [[residual_codebook]]
- Think of how I can apply forecasting algorithm during the base session.
- Vector quantization to the residual prototypes? Or just use K-means clustering.
  > [!note]
  > Idea : [[k-means_prototypes#2025/04/16]]

## 2025/04/17

### Code-2025/04/17
- [x] Time to look at the embeddings and cos similarity of the prototypes.
  > [!note]
  > [[cosine_similarity_prototypes#2025/04/17]]

### Experiments-2025/04/17
- [x] Same setting with [[Experiments#2025/04/16]], but lowered `loss_a_freq`, `loss_res`.
  > [!note]
  > [[Experiments#2025/04/17]]

### Idea-2025/04/18
- [[confusing_keywords#2025/04/16]]

### Tomorrow-2025/04/17
- Using the residual energy(as a scaling factor).

## 2025/04/18

### Experiments-2025/04/18
- [x] Same setting with [[Experiments#2025/04/17]], `loss_lpc` and `loss_lpc_shuffled` 1.
  > [[Experiments#2025/04/18]]

### Idea-2025/04/18
- [[confusing_keywords#2025/04/18]]

## 2025/04/19

### Idea-2025/04/19
- Need to apply all the ideas to the code and experiment.
- It can be a good timing to think about 2d embeddings(the embeddings of the model before the pooling layer).

### Experiments-2025/04/19
- [ ] Check the performance without Transformer layer.
  > [!note] Things to experiment
  > - Loss with LPC related features.
  > - 
