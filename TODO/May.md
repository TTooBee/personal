---
id: May
aliases: []
tags: []
---

# May

## 2025/05/01

### Study-2025/05/01
- [x] Signal and Systems.
  - Z-transform.

### Experiments-2025/05/01
- [x] Base code train with 100 epochs(same setting with [[Experimetns-2025/04/30]]).
> Result : [[Experiments#2025/05/01]]
> Server : [[Server#2025/05/01]]
- [x] Set all the LPC related parameters to 1 and test the code.
> Result : Do this with a single epoch to test the code.
> Server : [[Server#2025/05/01]]
- [x] Set LPC classification loss(`loss_a_freq_cls`, `loss_res_cls`) to 1.
> Result : [[Experiments#2025/05/01]]
- [x] Experiment with `comp` code(`noise_prob` 0.0).
> Result : [[Experiments#2025/05/01]]
> Server : [[Server#2025/05/01]]

### Code-2025/05/01
- [x] Write the code : `analysis/class_similarity.py`.
  > This code is to see the similarity between the base classes.
- [-] Write code that shows the similar classes.
  - Not just similar classes. Rank of similarity.

### Tomorrow-2025/05/01
- Experiment with `comp` code with `noise_prob` set to 0.8.

## 2025/05/02

### Idea-2025/05/02
- [-] Should think about evidence that formant and residual embeddings shape the vector space as I thought.

### Code-2025/05/02

### Experiments-2025/05/02
- [x] Experiment with `comp` code(`noise_prob` 0.8).
> Result : [[Experiments#2025/05/02]]
> Server : [[Server#2025/05/02]]

## 2025/05/03

### Idea-2025/05/03

### Experiments-2025/05/03
- [x] Set `loss_aug_a_freq` and `loss_aug_res` to 1.
> Result : [[Experiments#2025/05/04]]
> Server : [[Server#2025/05/03]]

## 2025/05/04

### Experiments-2025/05/04
- [x] Set `loss_aug_a_freq` and `loss_aug_res` to 1.
> Result : [[Experiments#2025/05/04]]
> Server : [[Server#2025/05/04]]

## 2025/05/07

### Code-2025/05/07
- [-] Now it's time to make my idea work with the code.
  > Code : [[manifold_mixup_lpc#2025/05/07]]

### Experiments-2025/05/08
- [x] Experiment with [[#Code-2025/05/07]], 30 epochs.
  > Result : [[Experiments#2025/05/07]]
  > Server : [[Server#2025/05/07]]

## 2025/05/08

### Others-2025/05/08
- [x] Order a cake.
- [x] Call mom and dad.
- [x] Call Adidas.

### Code-2025/05/08
- [x] Make the code work without a pre-trained model.
- [-] Apply the other methods.
  - [x] 1. Use the proposed method at the beginning(not at the middle of the base session).
  - [ ] 2. Use feature generator to make dummy classifier.
    - Think this would work pretty well.
  - [ ] 3. Get speech embedding loss with dummy classifier(the current code gets loss only with `fc`).
  - [ ] 4. Use many different classes for augmentation(the current code use only one class for augmentation).

### Experiments-2025/05/08
- [x] [[manifold_mixup_lpc#2025/05/07]] without using pre-trained model, 10 epochs.
  > Result : [[Experiments#2025/05/08]]
  > Server : [[Server#2025/05/08]]
- [x] [[manifold_mixup_lpc#2025/05/07]] without using pre-trained model, 30 epochs.
  > Result : [[Experiments#2025/05/08]]
  > Server : [[Server#2025/05/08]]
- [x] [[manifold_mixup_lpc#2025/05/07]] without using pre-trained model, 100 epochs.
  > Result : [[Experiments#2025/05/08]]
  > Server : [[Server#2025/05/08]]
- [x] Number 1 in [[#Cdoe-2025/05/08]], 10 epochs.
  > Result : [[Experiments#2025/05/08]]
  > Server : [[Server#2025/05/08]]
- [x] Number 1 in [[#Cdoe-2025/05/08]], 30 epochs.
  > Result : [[Experiments#2025/05/08]]
  > Server : [[Server#2025/05/08]]
- [x] Base train with `load_from_file` set to 1 to get the result of formant and residual embeddings.
  - I have a base model in the last experiment on 2025/04/30, so just train this model with 0 epoch
    and just get the result.
- [x] Number 1 in [[#Code-2025/05/08]], 100 epochs.
  > Result : [[Experiments#2025/05/08]]
  > Server : [[Server#2025/05/08]]

### Study-2025/05/08
- [ ] LUPI(Learning Under Privileged Information) paper.
  > Detail : [[LUPI#2025/05/08]]
- [ ] Multi-task learning in speaker verification.
  > Detail : [[multi-task_learning_in_speaker_verification#2025/05/08]]
> [!note]
> Two papers above are closely related to why the model with LPC classification loss performs
> better than the base model.

## 2025/05/09

### Others-2025/05/09
- [ ] Look at what I should do with Hyper-Hanyang.

### Code-2025/05/09
- [-] Apply the other methods.
  - [x] ~~1. Use the proposed method at the beginning(not at the middle of the base session).~~
  - [x] 2. Use feature generator to make dummy classifier.
    - Think this would work pretty well.
    > Detail : [[manifold_mixup_lpc#2025/05/09]]
  - [x] 3. Get speech embedding loss with dummy classifier(the current code gets loss only with `fc`).
  - [ ] 4. Use many different classes for augmentation(the current code use only one class for augmentation).

### Experiments-2025/05/09
- [x] Experiment with number 2 in [[#Code-2025/05/09]], 10 epochs.
  > Result : [[Experiments#2025/05/09]]
  > Server : [[Server#2025/05/09]]
- [x] Experiment with number 2 in [[#Code-2025/05/09]], 100 epochs.
  > Result : [[Experiments#2025/05/09]]
  > Server : [[Server#2025/05/09]]
  > [!fail]  Stopped at 30th epoch.
- [x] Experiment with number 3 in [[#Code-2025/05/09]], 10 epochs.
  > Result : [[Experiments#2025/05/09]]
  > Server : [[Server#2025/05/09]]
- [x] Experiment with number 3 in [[#Code-2025/05/09]], 10 epochs, without feature generator.
  > Result : [[Experiments#2025/05/09]]
  > Server : [[Server#2025/05/09]]
- [x] Experiment with number 3 in [[#Code-2025/05/09]], 100 epochs, with one method from the two above
  that shows better result.
  > The result from the experiment without the feature generator is better in 10 epochs training.
  > Result : [[Experiments#2025/05/09]]
  > Server : [[Server#2025/05/09]]

### Idea-2025/05/09
- Think about [[confusing_keywords#Projection vector of the speech embedding to the vector span of the formant and residual embedding-2025/05/09]].

### Tomorrow-2025/05/09
- Tomorrow or today, make the code work with the first idea in [[#Idea-2025/05/09]].

## 2025/05/10

### Experiments-2025/05/10
- [x] Experiment with number 3 in [[#Code-2025/05/09]], 30 epochs, without feature generator.
  > Result : [[Experiments#2025/05/10]]
  > Server : [[Server#2025/05/10]]

### Idea-2025/05/10
- [ ] Think how I should use structural vector.
  > Code : [[manifold_mixup_lpc#Structural Vector-2025/05/10]]

### Idea-2025/05/09
- Think of [[confusing_keywords#2025/05/10]].

## 2025/05/12

### NeoVim-2025/05/12
- [x] Fix the LSP issue with Claude and GPT.
  - Thank you Claude.

### Others-2025/05/12
- [x] Try NotebookLM.
- [x] [Alacritty-smooth-cursor](https://github.com/GregTheMadMonk/alacritty-smooth-cursor).
- [-] Hyper-Hanyang.
  - Should attach the picture of mail or message sent to the prof.

### Code-2025/05/12
- [x] Make the training code that uses structure vector.
  - I was making the function `get_structure_vector` in `manifold_mixup_test.py` work with batch.
  > Code : [[manifold_mixup_lpc#structure_vector-2025/05/12]]

### Idea-2025/05/12
- Think of how to use structure vector during the training.

### Experiments-2025/05/12
- [x] Experiment with [[manifold_mixup_lpc#structure_vector-2025/05/12]], 10 epochs.
  > Result : [[Experiments#2025/05/12]]
  > Server : [[Server#2025/05/12]]
- [x] Experiment with [[manifold_mixup_lpc#structure_vector-2025/05/12]], 50 epochs.
  > Result : [[Experiments#2025/05/12]]
  > Server : [[Server#2025/05/12]]

## 2025/05/13

### Others-2025/05/13
- [ ] Make the automated code tracker with Claude.

### Code-2025/05/13
- [x] Make the code work with only one additional dummy classifier that reserves the space for
  the keyword "tree".
  - Let's merge the `develope` branch to `feature/lpc` branch, and then do the rest of the work
    in the `develope` branch.
  > Detail : [[manifold_mixup_lpc#Single dummy classifier-2025/05/13]]
- [-] Make the code that uses the structure vectors with the span of the formant embedding and the
  new residual embedding.
  - Also think about using lamdas to make a new virtual vectors.
  > Detail : [[manifold_mixup_lpc#Using new structure vector-2025/05/13]]
  > NaN occurs with this code.
- [x] Should try the method used in FACT.
  - I should try dual labeling.
  > Detail : [[Forward_Compatible_Few-Shot_Class-Incremental_Learning]]
  > Code : [[FACT#2025/05/13]]
- [ ] Revise the code. So dirty.
  - Make the whole process to a single function.
  - From embeddings -> get logits -> smoothing -> get loss.
  - Maybe tomorrow...

### Experiments-2025/05/13
- [x] Experiment with [[#Code-2025/05/13]], 10 epochs, match "three" with "eight".
  > Result : [[Experiments#2025/05/13]]
  > Server : [[Server#2025/05/13]]
- [x] Experiment with [[#Code-2025/05/13]], 10 epochs, match "three" with "two".
  > Result : [[Experiments#2025/05/13]]
  > Server : [[Server#2025/05/13]]
- [x] Experiment with [[#Code-2025/05/13]], 10 epochs, match "three" with "two", use lamdas.
  > Result : [[Experiments#2025/05/13]]
  > Server : [[Server#2025/05/13]]
- [x] Experiment with FACT, set `loss_res_replace` and `loss_res_replace_fact` to 1.
  > Result : [[Experiments#2025/05/13]]
  > Server : [[Server#2025/05/13]]
- [x] Experiment with FACT, set `loss_res_replace` 0, `loss_res_replace_fact` to 1.
  > Result : [[Experiments#2025/05/13]]
  > Server : [[Server#2025/05/13]]
- [x] ~~Experiment with FACT, set `loss_res_replace` 0, `loss_res_replace_fact` to 1,~~
  ~~`epoch_guideline` 10.~~
  > Result : [[Experiments#2025/05/13]]
  > Server : [[Server#2025/05/13]]
  > Stopped.
> [!note]
> Projects/FSKIL/checkpoint/gsc2/trans/ft_cos-avg_cos-data_init-start_0/0514-00-25-34-010-Epo_1-Bs_128-sgd-Lr_0.005-decay0.0005-Mom_0.9-Max_100-NormF-T_16.00 was the last saved result.

## 2025/05/14

### Experiments-2025/05/14
- [x] Experiment with [[manifold_mixup_lpc#structure_vector-2025/05/12]], 100 epochs.
  > Result : [[Experiments#2025/05/14]]
  > Server : [[Server#2025/05/14]]
- [x] Fine tune the model below(LPC classification loss based model), 1 epoch.
  - Only with a single dummy classifier.
  > [!note]
  > Result : [[Experiments#2025/05/14]]
  > Server : [[Server#2025/05/14]]
  > Model path : [Path to the model](/home/sehyun/Projects/FSKIL/checkpoint/gsc2/trans/ft_cos-avg_cos-data_init-start_0/0514-13-43-07-056-Epo_0-Bs_128-sgd-Lr_0.005-decay0.0005-Mom_0.9-Max_100-NormF-T_16.00/session5_max_acc.pth)
- [x] Use `get_least_similar_emb_idx`, set `loss_res_replace` to 1, 10 epochs.
  > Result : [[Experiment#2025/05/14]]
  > Server : [[Server#2025/05/14]]
- [x] Experiment with [[manifold_mixup_lpc#Least and most similar embeddings-2025/05/14]], 10 epochs.
  > Result : [[Experiments#2025/05/14]]
  > Server : [[Server#2025/05/14]]
- [x] Experiment with [[manifold_mixup_lpc#Least and most similar embeddings-2025/05/14]], 10 epochs.
  > Result : [[Experiments#2025/05/14]]
  > Server : [[Server#2025/05/14]]

### Code-2025/05/14
- [x] During the second experiment in [[#Experiments-2025/05/14]], revise the code.
- [x] Freeze `fc_residual`.
  > Detail : [[freeze_parameter#2025/05/14]]
- [x] Make virtual embeddings with classes that have similar formant.
  > Detail : [[manifold_mixup_lpc#Least and most similar embeddings-2025/05/14]]

## 2025/05/15

### Others-2025/05/15
- [x] Meeting material.

### Experiments-2025/05/15
- [x] Experiment with [[manifold_mixup_lpc#Least and most similar embeddings-2025/05/14]], with `emb_replace_mode`
  set to `sturct`, 100 epochs.
  > Result : [[Experiments#2025/05/15]]
  > Server : [[Server#2025/05/15]]
  > Stopped. `emb_replace_mode` was set to "add".
- [x] Experiment with [[manifold_mixup_lpc#Least and most similar embeddings-2025/05/14]],
  without using least similar residual embeddings for augmentation, 100 epochs.
  > Result : [[Experiments#2025/05/15]]
  > Server : [[Server#2025/05/15]]
  > The model below is the file that is extracted in the middle of the training, and it shows a good result.
  > It's better than the final result.
  > [Path to the model](/home/sehyun/Projects/FSKIL/checkpoint/gsc2/trans/ft_cos-avg_cos-data_init-start_0/0515-15-03-45-130-Epo_0-Bs_128-sgd-Lr_0.005-decay0.0005-Mom_0.9-Max_100-NormF-T_16.00)
- [x] LPC classification loss with current code, 100 epochs.
  > Result : [[Experiments#2025/05/15]]
  > Server : [[Server#2025/05/15]]

### Code-2025/05/15
- [x] Modify `get_least_or_most_similar_emb_idx`.
  - The function will choose the same class's embedding when choosing the most similar embedding
    in the current code. So use `train_label` and mask the same class's embeddings' similarity.
- [ ] Insert the incremental session in the middle of the base training session.

## 2025/05/16

### Code-2025/05/16
- [-] Insert the incremental session in the middle of the base training session.
  > Detail : [[insert_incremental_session#2025/05/16]]
  > Solve the error related to the seen performance drop.

### Idea-2025/05/16
- Use residual energy as a structure vector of formant and residual.
  - Then I'll need a residual signal in time domain.
  > Detail : [[confusing_keywords#Residual energy-2025/05/16]]

### Experiments-2025/05/16
- [ ] Experiment with [[insert_incremental_session#2025/05/16]], 30 epochs.

## 2025/05/17

### Code-2025/05/17
- [-] Insert the incremental session in the middle of the base training session.
  > Detail : [[insert_incremental_session#2025/05/17]]
  > Solve the error related to the seen performance drop.
- [ ] Use residual energy as a structure vector.
  > Idea : [[confusing_keywords#Residual energy-2025/05/16]]
  > Code : [[manifold_mixup_lpc#Use residual energy-2025/05/17]]

## 2025/05/18

### Code-2025/05/18
- [-] Insert the incremental session in the middle of the base training session.
  > Detail : [[insert_incremental_session#2025/05/18]]
  > Taking time problem solved.
- [ ] Use residual energy as a structure vector.
  > Idea : [[confusing_keywords#Residual energy-2025/05/16]]
  > Code : [[manifold_mixup_lpc#Use residual energy-2025/05/17]]

### Experiments-2025/05/18
- Experiment base, trans after [[insert_incremental_session#2025/05/18]].

## 2025/05/19

### Code-2025/05/19
- [x] Insert the incremental session in the middle of the base training session.
  > Detail : [[insert_incremental_session#Matrix shape mixmatch-2025/05/19]]
  > Taking time problem solved.
  - [x] Fix the error related to GUI.
  > [!success] Solved!
- [-] Use residual energy.
  - [x] Make a test code in `analysis/waveform_plot.py`.
  > Detail : [[confusing_keywords#Residual energy-2025/05/19]]
  - [ ] Check the correlation of the structure vector made with residual energy.

### Experiments-2025/05/19
- [x] Experiment with [[insert_incremental_session#Matrix shape mixmatch-2025/05/19]],
  only use LPC classification loss, 30 epochs.
  > Result : [[Experiments#2025/05/19]]
  > Server : [[Server#2025/05/19]]
- [x] Experiment with [[insert_incremental_session#Matrix shape mixmatch-2025/05/19]], base, 30 epochs.
  > Result : [[Experiments#2025/05/19]]
  > Server : [[Server#2025/05/19]]
- [x] Experiment with [[insert_incremental_session#Matrix shape mixmatch-2025/05/19]],
  residual replace, 30 epochs.
  > Result : [[Experiments#2025/05/19]]
  > Server : [[Server#2025/05/19]]

### Idea-2025/05/19
- Use residual energy as a structure vector. Use Fourier transform.
  > Idea : [[confusing_keywords#Residual energy-2025/05/19]]
  > Code : [[manifold_mixup_lpc#Use residual energy-2025/05/19]]

## 2025/05/20

### Code-2025/05/20
- [-] Make the code train the model with the residual energy.
  > Detail : [[manifold_mixup_lpc#Use residual energy-2025/05/20]]

## 2025/05/21

### Code-2025/05/21
- [ ] Cepstrum analysis.
  > Detail : [[cepstrum#2025/05/21]]

## 2025/05/22

### Code-2025/05/22
- [x] Cepstrum analysis.
  > Detail : [[cepstrum#2025/05/22]]
- [x] Use this code in during training.
  - Code works well with MFCC.
- [x] 15:45:27 : Use attention mechanism with formant, excitation, and speech embeddings.
  > Detail : [[form_exc_attention#2025/05/22]]

### Idea-2025/05/22
- 15:45:20 : Maybe I can use attention mechanism with formant, excitation, and speech.

### Experiments-2025/05/22
- [x] 16:35:47 : Experiment with [[form_exc_attention#2025/05/22]], base only with attention.
  > Result : [[Experiments#2025/05/22]]
  > Server : [[Server#2025/05/22]]
  > Stopped.
- [x] Turn all the attention off, and experiment with residual replace, 10 epochs.
  > Result : [[Experiments#2025/05/22]]
  > Server : [[Server#2025/05/22]]
  > Stopped.

### Tomorrow-2025/05/23
- 01:01:31 : Get cosine similarity of the adjacent(next) embedding, and compare it with spectrogram.
  - 01:03:39 : Then you can see whether the vectors are considering the phoneme or not.

## 2025/05/23

### Code-2025/05/23
- [x] 11:52:01 : Spectrum analysis code(`analysis/spectrum_analysis.py`).
  > 13:52:11 : Detail : [[manifold_mixup_lpc#Spectrum boundary detection-2025/05/23]]
- [-] 15:07:01 : Use [[manifold_mixup_lpc#Spectrum boundary detection code-2025/05/23]] in the training code.
- [x] 16:00:02 : Make the model use 100 coefficients as the input(it 40 now).
  > Detail : [[manifold_mixup_lpc#Spectrum boundary detection-2025/05/23]]

## 2025/05/24

### Code-2025/05/24
- [x] 10:32:15 : Make code gets the energy of the speech.
  > Detail : [[manifold_mixup_lpc#Spectrum boundary detection-2025/05/24]]
- [x] Moved all the functions in `helper.py` to `utils.py`.

## 2025/05/25

### Code-2025/05/25
- [x] 16:36:50 : Fix the problem of [[#Idea-2025/05/25]].
- [x] 19:06:16 : Make code gets the energy of the speech.
  > Detail : [[manifold_mixup_lpc#Spectrum boundary detection-2025/05/24]]

### Idea-2025/05/25
- 15:51:43 : Loss doesn't decease. Should think about how I can make this work.
  > [!success] Solved!
  - Should also think about the speed of the process.

### Experiments-2025/05/25
- [x] 23:33:43 : Experiment with [[manifold_mixup_lpc#Spectrum boundary detection-2025/05/24]].
  > Result : [[Experiments#2025/05/25]]
  > Server : [[Server#2025/05/25]]

## 2025/05/26
- 10:08:40 : Make code work fast. Save the peak indices as file.
- 10:08:43 : See why the performance of my algorithm is bad.
- 10:11:40 : Make FACT and TEEN work in my project.
> [!note]
> 10:15:21 : Should fix the code that loads the data from the dataloader(`base`, `comp`, `teen`).
> 10:15:52 : Or maybe I can make all the code work in trans code.

### Others-2025/05/26
- [ ] 10:36:40 : TA due : 2025/05/28 Wed.

### Code-2025/05/26
- [ ] Save the peak indices as file.
- [x] TEEN.
  - This is made a long time ago. Just fix the part related with dataloader.
- [-] FACT.
  - 11:24:12 : The code only saves the overall accuracy. Should make it able to show the seen
    and unseen accuracy separately.

### Idea-2025/05/26
- See why the performance of my algorithm is bad.

# 11:32:15
> [!note]
> You should do something with `test_withfc` after lunch.
> `test_withfc` is commented in `fscil_trainer`. Should use it.
