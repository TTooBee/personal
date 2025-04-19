---
id: March
aliases: []
tags: []
---

# Thing to do

- [ ] Never tried
- [x] Finished
- [-] Ongoing

## 2025-03-10

### About Univ-20250310

- [ ] Finish professor related paper.
- [ ] Gather all the papers.

### Code-20250310

- [x] Change CKA loss to something else that can consider the temporal features.
  - Changed it to 1 on 1 cosine similarity loss and the result was damn bad.
- [x] Little mistake with tensor dimensions. When shrinking the sequence length of the matrix embedding, should consider the row-major manners.
- [x] Distance based loss.
  - Almost the same as the angle based loss.
- [x] Attention based loss(every vector in the matrix looks at each other).

### Paper-20250310

#### Deep Learning

- [x] Wav2Vec
- [ ] Wav2Vec 2.0

#### Keyword Spotting

- [ ] Relational Proxy ...

## 2025-03-11

### Code-20250311

- [ ] Use 2d loss in vector based learning process.
  - Apply attentive pooling to make the vector embeddings.
- [x] Apply attention-mechanism in bcresnet.py. [[Experiments#2025-03-10]]
  - Attention + 2d only = low performance.
  - Attention + 2d + vec = Still doesn't seem like it got better.
- [x] Use CTC loss just as the supplementary loss, and don't use CTC to get logits.
- [x] Matrix_loss_metric fixed. It was not being applied in the actual code.
- [x] Vector based, use two supplementary matrix based losses(attention + CKA)
  - It may be a good idea to make the argument 'matrix_loss_metric' a list to combine the losses.
- [ ] Maybe I should make a code to visualize the similarity between all the classes.

## 2025-03-12

### Code-20250312

- [ ] Use SSL(Wav2Vec 2.0 maybe) instead of BC-ResNet

## 2025-03-13

### Code-20250313

- [ ] Increase the embedding size and check if it makes performance better.
- [x] Check the vector embeddings without the transformer layers, vector based.
  - Primitive with only vector without transformer layers doesn't work well with unseen data. Should fix this.
  - Don't know what had been changed, but it's fixed now.

### Paper-20250313

- [ ] FLEXIBLE KEYWORD SPOTTING BASED ON HOMOGENEOUS AUDIO-TEXT EMBEDDING

## 2025-03-14

### Code-20250314

- [x] Fix the problem with low performance of the model learned without transformer layers. [[unseen_performance_drops#2025-03-14]]
- [x] After the problem above gets fixed, add the transformer layers that makes the structural embedding of the keyword. [[structural_embedding#2025-03-14]]
- [ ] Add formant and residual extracting code. [[formant_residual]]

## 2025-03-16

### Univ Things-20250316

- [x] Mail from Ko?
  - Finish the online education video, send the mail.

### Code-20250316

- [x] Make code that gets the differences between the adjacent vectors.

## 2025-03-17

### Code-20250317

#### Fix-20250317

- [x] Should make the code to use flexible channel size between 256 and 512. [[transformer_module#2025-03-17]]

#### Experiments-20250317

- [x] Two things compared. [[transformer_module#2025-03-17]], [[experiments#Test Result-20250317]]
  - Vector based only in trans code.
  - Vector based + Transformer module in trans code.

### Paper-20250317

- [ ] [FLEXIBLE_KEYWORD_SPOTTING_BASED_ON_HOMOGENEOUS_AUDIO-TEXT_EMBEDDING]

## 2025-03-18

### NeoVim-20250318

- [ ] Search for some ways to get rid of the markdown related warnings.
  - Or activate the rendering system of horizontal line.

### Code-20250318

#### Feature-20250318

- [-] Get formant and residual, and save them into .pt file? [[formant_residual#2025-03-14]]
  - Think this should be done without noise.
  - ==Practice code made. This function should be the default linear prediction code.==

#### Experiments-20250318

- [x] GRU 100 epochs. [[experiments#Test Result-20250318]]
- [x] Transformer 1 layer 100 epochs. [[experiments#Test Result-20250318]]
- [x] Set `noise_prob` to 0.0 and train all the algorithms again. [[experiments#Test Result-20250318]]
  - [x] None [[experiments#Test Result without noise-20250319]]
  - [x] Transformer [[experiments#Test Result without noise-20250319]]
  - [x] GRU [[experiments#Test Result without noise-20250319]]

#### Fix-20250318

- [x] Make GRU and Transformer layers optional. [[transformer_module#2025-03-18]]

### Paper Review-20250318

- [-] Paper review...

## 2025-03-19

### Code-20250319

#### Experiments-20250319

- [x] Increase the channel from 256 to 512 and check the performance of vector based only code both with noise and without noise. [[Experiments#2025-03-19]]
- [x] One GRU layer without noise. [[experiments#Test Result without noise-20250319]]

#### Fix-20250319

- [x] Increase the channel of BC-ResNet from 256 to 512 to check whether the difference of the performance is from channel size or not. [[channel_size_increase#2025-03-19]]

## 2025-03-20

### NRL-20250320

- [>] Understand the basics.

### Code-20250320

#### Feature-20250318

- [ ] Finish the code extracts lpc and residual signal. [[formant_residual]]

#### Experiments-20250320

- [x] Increase the last embedding size only to 512. [[channel_size_increase#2025-03-20]], [[Experiments#2025-03-20]]
- [x] Normal embedding size(256), set `loss_regularize` to 0(both with noise and without noise). [[Experiments#2025-03-20]]
  - [x] Transformer.
  - [x] GRU.

## 2025-03-21

### Code-20250321

#### Experiments-20250321

- [x] Set `loss_regularize` to 0, embedding size 256, Transformer and GRU. [[experiments#2025-03-21]]

#### Feature-20250321

- [ ] Should match the python code and the matlab code that extracts the formant and LP residual. [[lpc#2025-03-21]]
  - [ ] All I need to do is to make the code that extracts the right formant from LPC.
- [x] Install Matlab on Ubuntu.

## 2025-03-22

### Idea-20250322

- [ ] Search for information about my idea. [[sound_play#2025-03-22]]

### Experiments-20250322

- [x] Baseline experiments. [[Experiments#2025-03-22]]
  - [x] `base` without noise.
  - [x] `base` with noise.
  - [x] `comp` without noise.
  - [x] `comp` with noise.

## 2025-03-23

### Code-20250323

- [>] Extract LPC and residual. [[lpc#2025-03-23]]

## 2025-03-24

### Code-20250324

- [x] Find why subtracting 100000 from the formant works. [[lpc#2025-03-24]]
  - 1e6 was being added, not 1e-6.

## 2025-03-25

### Experimental Acoustic-20250325

- [ ] Extract the formant(F1, F2) of **'/ㅏ/, /ㅣ/'** of the voice of three males and 3 females(there will be audio files) with Praat, and compare the result of males and females.

### Code-20250325

- [x] Finish it...!!! Make code that extracts spectrogram and lpc related things.
- [x] Save them in pt files. [[lpc#2025-03-25]]

## 2025-03-26

### NRL-20250326

- [x] Make tables for detailed number.
  - [x] Global tech.
  - [x] Education.
  - [x] Scholar.
  - [x] Equipments.
  - [x] Professors.
  - [x] Post Doc.
  - [x] Short term, long term internship.

### Code-20250326

- [x] Extract LPC related features and saved them in separated directories.

## 2025-03-27

### Lab-20250327

- [x] Team meeting materials.

### Code-20250327

- [x] Make code that loads the pt files to get spectrum and other LPC related things. [[lpc#2025-03-27]]
- [x] Use LPC!! [[lpc#2025-03-27]]
- [x] NaN Embedding..? [[load_from_file_nan#2025-03-27]]
  - **Solved!**

## 2025-03-28

### Experiments-20250328

- [x] `lpc_loss 1`. [[Experiments#2025-03-28]]
- [x] `lpc_loss 0`. [[Experiments#2025-03-28]]
- [x] `lpc_loss 1`, without shuffling the order of speech, res, formant. [[Experiments#2025-03-28]]

### Code-20250328

- [x] Should remove all the parts related to 2d embeddings. [[remove_2d#2025-03-28]]
- [x] Don't shuffle the order of speech, formant, residual. Think this should be experimented earlier than shuffling method.
  [[lpc#2025-03-28]] [[#Experiments-20250328]]
- [ ] Extract lpc related features in noisy speech(`noise_prob 0.8`).

### Idea-20250328

- [ ] Why not apply SpecAugment to the residual and the formant?

## 2025-03-29

### Code-20250329

- [ ] Should make code that applies manifold mixup with lpc related things.

### Experiments-20250329

- [x] `gsc2.py` modified. Now it can apply noise. [[Experiments#2025-03-29]]
- [x] Apply noise(0.8) **only to speech**, and match the speech embeddings with formant and residual embeddings. [[Experiments#2025-03-29]]
- [x] Set `lpc_loss` 0, `noise_prob` 0.0 with loaded pt files. [[Experiments#2025-03-29]]

## 2025-03-31

### Idea-20250331

- [ ] Should make how to make the compositional information with LPC.
  [[compositional_lpc#2025-03-31]]
- [ ] Should try to find the pattern of the misclassification to make the performance better.
  - [ ] I also want to know the output of the model in the point of formant and residual embeddings.
  - Would there be any pattern between the speech, formant, and residual embeddings?

### Code-20250331

- [x] Add Shuffling code for speech, formant, and residual. [[lpc#2025-03-31]]
- [ ] Make the code that can apply the forward compatible learning  with formant and residual embeddings. [[Forward_Compatible_Few-shot_Class-incremental_Learning]]
  - [x] Make code that saves the prototypes of the formant and residual embeddings.
  [[formant_residual_prototype#2025-03-31]]

### Experiments-20250331

- [x] Set `lpc_loss` to 0, `noise_prob` 0.8 with loaded pt files.
- [x] Set `lpc_loss` to 0, `noise_prob` 0.0 with loaded pt files.
- [x] Set `lpc_loss` to 0.5, and `lpc_loss_shuffled` to 0.25. [[Experiments#2025-03-31]]
