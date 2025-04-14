---
id: lpc
aliases: []
tags: []
---

# lpc

## 2025-03-18
```python code used to extract lps and residual insdie helper.py(don't try to use this directly)
        waveform = waveform.detach().to("cpu")
        window_size = 320
        lpc_order = 16

        est_x = []
        res = []
        for batch in range(waveform.shape[0]):
            est_x_batch = []
            res_batch = []
            for i in range(waveform.shape[2] // window_size):
                frame = waveform[batch, :, window_size * i : window_size * (i + 1)]
                if i == 0:
                    frame = torch.cat(
                        (
                            torch.zeros(waveform.shape[1], lpc_order),
                            frame,
                        ),
                        dim=1,
                    )
                else:
                    frame = torch.cat(
                        (
               )
                res_batch.append(res_frame)

            est_x_batch_tensor = torch.cat(est_x_batch, dim=1)
            res_batch_tensor = torch.cat(res_batch, dim=1)

            est_x.append(est_x_batch_tensor)
            res.append(res_batch_tensor)

        est_x_tensor = torch.cat(est_x, dim=0)
        res_tensor = torch.cat(res, dim=0)

        hop_length = 160
        win_length = 480
        n_fft = 512
        n_mels = 40
        sample_rate = 16000
        mel_spectrogram = torchaudio.transforms.MelSpectrogram(
            sample_rate=sample_rate,
            hop_length=hop_length,
            n_fft=n_fft,
            win_length=win_length,
            n_mels=n_mels,
        )
        spec_est_x = mel_spectrogram(est_x_tensor.to(torch.float32) + 1e-6)
        spec_res = mel_spectrogram(res_tensor.to(torch.float32) + 1e-6)
             waveform[
                                batch, :, window_size * i - lpc_order : window_size * i
                            ],
                            frame,
                        ),
                        dim=1,
                    )
                # shape of frame : (1, window_size + lpc_order)
                frame = frame.numpy()
                a = librosa.lpc(frame[0], order=lpc_order)
                a_1 = np.concat((np.zeros(1), -1 * a[1:]))

                est_frame = scipy.signal.lfilter(a_1, 1, frame)
                res_frame = frame - est_frame

                est_frame = torch.tensor(est_frame[:, lpc_order:])
                res_frame = torch.tensor(res_frame[:, lpc_order:])

                est_x_batch.append(est_frame)
        # ...
```
- Should use this code on data level(not on the code level).
- Tricky part is, the original code gets the spectrogram as the deep learning code gets run. If LPC is done like this the code becomes crazily slow
- So it should be prepared before the learning process starts, but then it's not actually able to apply noise augmentation. 

```python lpc.py in practice folder
def get_lp_residu(audio_file_path, lpc_order, window_size):
    waveform, sample_rate = librosa.load(audio_file_path, sr=None)
    res_list = []
    a_list = []
    for i in range(waveform.shape[0] // window_size):
        frame = waveform[i * window_size : (i + 1) * window_size]
        if i == 0:
            frame_mem = np.concatenate((np.zeros((lpc_order)), frame), axis=0)
        else:
            frame_mem = np.concatenate(
                (waveform[i * window_size - lpc_order : i * window_size], frame), axis=0
            )
        a = librosa.lpc(frame_mem, order=lpc_order)
        _, a_freq = scipy.signal.freqz(1, a, worN=40)
        print(a_freq.shape)
        a_list.append(np.expand_dims(a, axis=1))
        a_1 = np.concat((np.zeros(1), -1 * a[1:]))
        est_frame = scipy.signal.lfilter(a_1, 1, frame_mem)
        est_frame = est_frame[lpc_order:]
        res_frame = frame - est_frame
        res_list.append(res_frame)
    res = np.concat(res_list, axis=0)
    a_all = np.concat(a_list, axis=1)

    return a_all, res
```
- Set `worN` to 40. Nothing to do else before putting this is to the model..?

## 2025-03-21
- The result of spectrogram and FFT is different between python and Matlab.
- Spectrogram works well in `Preprocess` code, but don't know why.
  - Maybe I can use `Preprocess` code to get the spectrogram of the residual?
  - Apply noise also when extracting the LPC and residual.
- Should window size of FFT and LPC be the same? Think it'll be hard, so let's just do this and make formant 2d.

## 2025-03-22
- Sequence length is always 101 regardless of the window size.

## 2025-03-23
- Finish it..!!
- Spectrogram of the residual and the speech can be extracted just by using `Preprocess` code, but the problem is formant.
- Sequence length of the lpc is now 50(and it can be set to 100 if the window size of lpc is set to 160), but not really sure how it can be set to 101. Zero pad and the end of the utterance of the formant?
- Think just padding at the end of the frame is the best I can try right now.
- I have the code that extracts the spec, res_spec, and lpc for a ==single audio file==, so just apply this code to all the audio files I have.
- Save all the data in .pt files? If the audio file name is `abcd.wav`, then spectrum of the residual is saved as `abcd.pt` in `keyword_residual` directory.
- Zero padding part added to set the sequence length of the formant to 101.

```python data/lpc.py
        # zero padding to make the sequence length of a_all and a_freq_all 101
        a_all = np.concat((a_all, np.zeros((a_all.shape[0], 1))), axis=1)
        a_freq_all = np.concat((a_freq_all, np.zeros((a_freq_all.shape[0], 1))), axis=1)
```

- Convert residual, formant to torch tensor.

```python data/lpc.py
a_all, a_freq_all, res = self.get_lp_residu(x, 10, 160)
        res = torch.tensor(res).to(torch.float32)
        res = torch.unsqueeze(res, dim=0)

        a_all = torch.tensor(a_all).to(torch.float32)
        a_all = torch.unsqueeze(a_all, dim=0)

        a_freq_all = torch.tensor(a_freq_all).to(torch.float32)
        a_freq_all = torch.unsqueeze(a_freq_all, dim=0)
```
- **Formant gets extracted properly when the value is subtracted by 1000000**. Could it be related to `dtype`?
  - Fixed! This 1e6 was being added, not 1e-6.

## 2025-03-24
- Think dtype is not the one I should fix.
- Should also apply mel-filterback to the formant.

## 2025-03-25
- Save pt files. Just like running the training code, make `gsc2_form_res.py`.
  - `.pt` files for **spec, res_spec, a, a_freq, target(label)** are saved in `FSKIL/data/datasets/GSC2/GSC2_torch_tensor/noise_0.0(and 0.8)`
- Think all I need to do is to add some codes in `__getitem__`.

## 2025-03-26
- Spectrum , residual spectrum, a, a freq extracted.
- Now I need to make code that actually uses the formant and residual.
  - Load from files if `args.load_from_file == 1`.
  - Make formant and residual embeddings, and compare them with the speech embedding.

## 2025-03-27
- There are several parts that get data and label from the batch, but now I'm using the code that the batch has spec, res_sec, etc, so there should be a conditional part when getting materials from the batch.
- Logits for residual embeddings and formant embeddings are not needed. Just need to get the embedding and compare them with the speech embeddings.

- There are several ways to make loss of `residual emb + formant emb = speech emb`.
  1. Calculate all the possible combination including the class itself.
  2. Calculate only the different sample's embeddings.
  3. Do number 2 randomly
- [[Experiments#2025-03-28]]

## 2025-03-28

### Non_shuffling-20250328

---
- The current code shuffles the order of speech, formant, and residual. This is being done by the code below.
```python helper.py shuffle_idx_by_class
def shuffle_idx_by_class(class_dic, train_label):
    for class_idx in range(len(class_dic)):
        random.shuffle(class_dic[class_idx])
    new_order = torch.empty(train_label.shape)
    for label_idx in range(train_label.shape[0]):
        label = train_label[label_idx]
        new_order[label_idx] = class_dic[label.item()][0]
        class_dic[label.item()].pop(0)

    return new_order
```
---

- And the code below is where it's actually implemented.

---
```python shuffing part
            new_order_formant = shuffle_idx_by_class(class_dic, train_label)
            new_order_residual = shuffle_idx_by_class(class_dic_copied, train_label)
```
---

- If this part is removed, then there is no shuffling. [[Experiments#2025-03-28]]

## 2025-03-31

### Shuffling_added-2025-03-31

---
```python helper.py base_train formant residual shuffled embedding
            new_order_formant = shuffle_idx_by_class(class_dic, train_label)
            new_order_residual = shuffle_idx_by_class(class_dic_copied, train_label)

            emb_a_freq_shuffled = emb_a_freq[new_order_formant.long(), :]
            emb_res_shuffled = emb_res[new_order_residual.long(), :]
```
```python helper.py base_train shuffled loss
  total_loss = (
                loss
                + args.loss_lpc * loss_lpc
                + 0.5 * args.loss_lpc * loss_lpc_shuffled
              )
```
---

- 0.5 is multiplied for the loss_lpc_shuffled, since the model performance dropped when only learned with loss_lpc_shuffled.

## 2025-04-04
- Temporarily added the line that makes `emb` to `emb_a_freq`.
  - Set `loss_lpc` to 0.

---
```python helper.py make model learn only with formant embedding
emb = emb_a_freq
```
---

## 2025/04/05
---
> [!important]
> Always double check the thing below.
> Find 'remove' or 'change' in helper.py and Network.py.
---
- The code should be changed in **FOUR** parts.
  1. `emb` in `base_train`, `helper.py`. [!check]
    > `data = emb_a_freq  # should remove this part`
  2. `final_logits` in `test`, `helper.py`. [!check]
    > `final_logits = logits  # should be changed`
  3. `embedding` in `replace_base_fc`, `helper.py`. [!check]
    > [!note]
    > It seems like I already made the code that extract the prototypes of LPC related data.
    > But maybe just need to change the code below.
    ---
    ```python helper.py
      embedding, _ = model(data) # change this to 'a_freq_data' or 'res_data'
      embedding_res, _ = model(res_data)
      embedding_a_freq, _ = model(a_freq_data)
    ```
    ---
  4. `data` in `update_fc`, `Network.py`. [!check]
  ---
    ```python Network.py
        def update_fc(self, dataloader, class_list, session):
        for batch in dataloader:
            if self.args.load_from_file == 1:
                data_ori, res_data, a, a_freq_data, label = [_.cuda() for _ in batch]
                # Here should be something that makes NaN 0 actually
            else:
                data_ori, label = [_.cuda() for _ in batch]
            data = self.encode(data_ori)[0].detach()
    ```
    ---
- Changed all the parts to make the model learn only with formants.
  > [!note] 
  > [April#2025/04/06]]
- An error related to thread occurred but think it's not what I should handle.
