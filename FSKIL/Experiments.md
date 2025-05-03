---
id: Experiments
aliases: []
tags: []
---

# Experiments

```
## 2025-03-23
```txt comp without noise
[98.546, 97.098, 95.023, 92.401, 90.001, 86.595]
Seen acc:
[98.192, 98.071, 97.567, 97.062, 95.776]
Unseen acc:
[82.628, 73.878, 68.824, 64.607, 59.779]
Harmonic mean:
[89.74, 84.273, 80.713, 77.577, 73.612]
```
|comp|base|form-res|
|--------------------|--------------------|--------------------|
|[Result](#base)|[Result](#2025-03-23)|[Result](#2025/04/09)|


## 2025-03-10

- [x] Lower the ratio of the 2d loss.
  - Should try some new things.
- [x] Poor performance for unseen classes when tested with **low 2d-loss** and **vector based test**.
- [-] Low 2d-loss and matrix based test.

## 2025-03-11

- [x] Vector based learning.
  - Vector based is much better than matrix based learning, so maybe vector based learning process should be the main and the matrix based should be minor.
  - Current code just do the global average pooling when making the embeddings. Why not use attention for this? [[attentive_pooling]]
  - Without 2d loss, without anything with matrix related things in testing and calculating loss, performance drops just because comp and primitive code are different? If so, this can be a kind of a good news..
  - When CKA is used alone, then it cannot consider the position of the vectors, so maybe I can use self-attention mechanism to make vectors that has contextual information. [[self_attention.md]]
- Vector based *vs* Matrix based

## 2025-03-12

- [x] Complete TSNE test code.
  - The vectors of the data(matrix) draw a line. This means that there could be a better result if the model can learn the certain pattern in this path.
  - See a lot of disconnections in the lines

## 2025-03-17

### Test Result-20250317
``` txt Vector based only in trans code
[97.409, 95.278, 92.448, 89.949, 86.445, 83.324]
Seen acc:
[96.992, 96.943, 96.461, 94.882, 93.598]
Unseen acc:
[72.656, 61.334, 60.052, 57.758, 53.863]
Harmonic mean:
[83.079, 75.133, 74.022, 71.805, 68.377]
```
``` txt Transformer module used(2 Transformer layers)
[97.305, 94.981, 92.508, 90.147, 87.147, 84.198]
Seen acc:
[97.076, 97.003, 96.559, 95.58, 94.571]
Unseen acc:
[67.366, 61.447, 60.839, 57.594, 54.769]
Harmonic mean:
[79.537, 75.236, 74.646, 71.877, 69.366]
```
### Test Result-20250318
```txt GRU module used
[97.217, 95.222, 92.569, 90.057, 86.555, 83.072]
Seen acc:
[96.84, 96.657, 96.104, 94.811, 93.028]
Unseen acc:
[73.833, 64.159, 62.377, 57.863, 54.525]
Harmonic mean:
[83.786, 77.124, 75.652, 71.866, 68.753]
```
```txt One Transformer layer
[97.177, 94.882, 92.418, 90.206, 86.68, 83.081]
Seen acc:
[96.589, 96.396, 95.868, 95.08, 93.92]
Unseen acc:
[72.552, 65.136, 64.547, 57.958, 53.217]
Harmonic mean:
[82.863, 77.741, 77.15, 72.017, 67.939]
```
### Test Result without noise-20250319
```txt Vector based only without noise
[96.967, 94.174, 90.97, 88.121, 84.554, 81.288]
Seen acc:
[96.145, 95.804, 95.213, 94.374, 93.216]
Unseen acc:
[68.266, 58.073, 55.947, 50.074, 46.345]
Harmonic mean:
[79.842, 72.313, 70.48, 65.431, 61.91]
```
```txt One Transformer layer without noise
[96.913, 94.128, 91.186, 88.42, 85.069, 81.793]
Seen acc:
[96.022, 95.718, 95.057, 94.355, 93.274]
Unseen acc:
[69.207, 59.785, 58.19, 52.01, 48.955]
Harmonic mean:
[80.439, 73.6, 72.189, 67.057, 64.21]
```
```txt Two GRU layers without noise
[97.107, 93.809, 90.475, 87.542, 84.522, 80.676]
Seen acc:
[96.067, 95.811, 95.414, 94.367, 92.786]
Unseen acc:
[63.782, 53.867, 51.753, 49.582, 46.906]
Harmonic mean:
[76.664, 68.962, 67.107, 65.008, 62.312]
```
```txt One GRU layer without noise
[97.29, 94.368, 91.008, 87.663, 84.207, 81.595]
Seen acc:
[96.22, 95.94, 95.206, 94.319, 93.302]
Unseen acc:
[69.792, 57.23, 53.364, 49.356, 47.004]
Harmonic mean:
[80.902, 71.693, 68.393, 64.802, 62.514]
```

## 2025-03-18
- Vector based only in trans code and transformer module added in trans code.
  - Performance drops when two Transformer layers are used. Try 1 layer only. [[#Test Result-20250318]]

## 2025-03-19
- Vector based only with increased channel size of BC-ResNet.
- If it's done by increasing `tau` to 16 from 8, then all the other size will be increased, so maybe it's better to change the last element of `self.c` to `base_c * 8`. This will change only the last embedding size of the model output.

```txt tau set to 16(embedding size 512) without noise
[96.958, 93.145, 89.67, 86.638, 82.531, 79.739]
Seen acc:
[96.053, 95.993, 95.429, 94.506, 93.414]
Unseen acc:
[54.502, 46.423, 46.52, 41.45, 39.864]
Harmonic mean:
[69.544, 62.581, 62.549, 57.626, 55.881]
```
```txt tau set to 16(embedding size 512) with noise
[97.29, 95.199, 92.685, 90.31, 87.317, 84.036]
Seen acc:
[96.968, 96.919, 96.474, 95.81, 94.479]
Unseen acc:
[72.046, 63.667, 62.207, 57.576, 54.31]
Harmonic mean:
[82.67, 76.85, 75.641, 71.928, 68.972]
```

```python bcresnet.py 
class BCResNets(nn.Module):
    def __init__(self, args, base_c, num_classes=12):
        super().__init__()
        self.args = args
        self.num_classes = num_classes
        self.n = [2, 2, 4, 4]  # identical modules repeated n times
        self.c = [
            base_c * 2,
            base_c,
            int(base_c * 1.5),
            base_c * 2,
            int(base_c * 2.5),
            base_c * 4,
        ]  # num channels
        self.s = [1, 2]  # stage using stride
        self._build_network()
```
## 2025-03-20

```txt Only the last embedding size increased(without noise)
[96.848, 93.867, 89.99, 86.944, 83.215, 80.243]
Seen acc:
[95.957, 95.673, 95.324, 94.365, 92.852]
Unseen acc:
[66.282, 50.94, 48.71, 44.406, 44.527]
Harmonic mean:
[78.406, 66.482, 64.474, 60.393, 60.19]
```
```txt Only the last embedding size increased(with noise)
[97.47, 95.028, 92.559, 90.087, 86.562, 83.27]
Seen acc:
[96.927, 96.758, 96.241, 94.024, 92.928]
Unseen acc:
[69.997, 63.673, 62.004, 60.404, 56.414]
Harmonic mean:
[81.29, 76.804, 75.419, 73.554, 70.207]
```
```txt Set loss_regularize to 0(Transformer without noise)
[97.159, 95.224, 92.453, 89.662, 85.957, 82.694]
Seen acc:
[96.611, 96.325, 95.771, 94.702, 93.58]
Unseen acc:
[77.172, 65.777, 61.949, 54.96, 51.609]
Harmonic mean:
[85.804, 78.173, 75.234, 69.554, 66.528]
```
```txt Set loss_regularize to 0(Transformer with noise)
[97.57, 95.618, 93.598, 91.555, 88.746, 85.405]
Seen acc:
[97.234, 97.112, 96.621, 95.217, 94.005]
Unseen acc:
[74.245, 69.291, 68.405, 65.405, 61.783]
Harmonic mean:
[84.199, 80.876, 80.101, 77.544, 74.562]
```

## 2025-03-21
```txt Set loss_regularize to 0(GRU without noise)
[97.232, 94.936, 92.033, 89.397, 86.373, 82.982]
Seen acc:
[96.915, 96.769, 96.215, 95.34, 94.22]
Unseen acc:
[69.147, 59.561, 58.442, 55.025, 50.859]
Harmonic mean:
[80.709, 73.737, 72.716, 69.778, 66.06]
```
```txt Set loss_regularize to 0(GRU with noise)
[97.385, 95.224, 92.72, 90.611, 87.732, 84.225]
Seen acc:
[96.987, 96.804, 96.349, 95.131, 93.512]
Unseen acc:
[72.373, 64.619, 64.529, 61.991, 58.627]
Harmonic mean:
[82.891, 77.503, 77.292, 75.066, 72.07]
```
## 2025-03-22

```txt base without noise
[97.683, 95.144, 92.192, 89.073, 85.921, 82.486]
Seen acc:
[97.168, 97.061, 96.677, 95.914, 94.472]
Unseen acc:
[68.861, 58.918, 54.675, 50.995, 48.349]
Harmonic mean:
[80.601, 73.326, 69.848, 66.587, 63.963]
```
```txt base with noise
[97.656, 95.608, 93.426, 91.426, 88.204, 85.027]
Seen acc:
[97.162, 97.063, 96.654, 96.261, 94.852]
Unseen acc:
[75.13, 68.244, 67.538, 59.837, 57.796]
Harmonic mean:
[84.737, 80.141, 79.514, 73.799, 71.826]
```
## 2025-03-23

```txt comp without noise
[98.546, 97.098, 95.023, 92.401, 90.001, 86.595]
Seen acc:
[98.192, 98.071, 97.567, 97.062, 95.776]
Unseen acc:
[82.628, 73.878, 68.824, 64.607, 59.779]
Harmonic mean:
[89.74, 84.273, 80.713, 77.577, 73.612]
```
```txt comp with noise
[98.436, 96.734, 94.518, 92.314, 88.598, 84.775]
Seen acc:
[98.07, 97.996, 97.551, 95.441, 93.203]
Unseen acc:
[79.236, 70.504, 68.439, 64.148, 60.706]
Harmonic mean:
[87.653, 82.007, 80.442, 76.726, 73.524]
```
## 2025-03-28
```txt formant residual loss_lpc 1 without noise
[97.317, 94.286, 90.915, 87.276, 83.6, 80.703]
Seen acc:
[96.813, 96.654, 96.294, 95.013, 93.891]
Unseen acc:
[61.167, 51.853, 46.445, 43.606, 41.991]
Harmonic mean:
[74.968, 67.496, 62.665, 59.777, 58.029]
```
```txt loss_lpc 0 without noise
[97.083, 94.62, 91.915, 89.095, 86.135, 82.721]
Seen acc:
[96.429, 96.186, 95.574, 94.848, 93.712]
Unseen acc:
[71.056, 62.617, 59.767, 55.449, 51.54]
Harmonic mean:
[81.821, 75.853, 73.544, 69.984, 66.504]
```
```txt loss_lpc 1 without noise, without shuffling the order of speech, res, formant, 10 epochs
[96.189, 93.844, 91.269, 88.646, 85.476, 82.27]
Seen acc:
[95.314, 94.592, 93.685, 92.779, 91.617]
Unseen acc:
[74.261, 68.35, 65.713, 59.616, 54.859]
Harmonic mean:
[83.481, 79.358, 77.245, 72.589, 68.626]
```

## 2025-03-29
```txt loss_lpc 1 with noise(0.8), without shuffling
[96.367, 93.799, 90.801, 88.421, 84.972, 82.117]
Seen acc:
[95.761, 95.432, 94.829, 93.784, 92.687]
Unseen acc:
[68.065, 58.917, 59.112, 54.764, 51.165]
Harmonic mean:
[79.572, 72.855, 72.827, 69.149, 65.933]
```
- Noise is only applied to the speech.
---
```python gsc.py
def load_pt_file(self, keyword, pt_file_name):
        if self.train and self.base_sess:
            noise_prob = self.args.noise_prob
        else:
            noise_prob = 0.0
        spec_pt_file_path = os.path.join(
            self.root,
            "GSC2",
            f"noise_prob_{noise_prob}",
            "keywords_spec",
            keyword,
            pt_file_name,
        )
        res_spec_pt_file_path = os.path.join(
            self.root,
            "GSC2",
            "noise_prob_0.0",
            "keywords_residual",
            keyword,
            pt_file_name,
        )
```
---
```txt loss_lpc 1 with noise(0.8) only on speech(50 epochs)
[96.287, 94.117, 91.164, 88.532, 84.969, 82.459]
Seen acc:
[95.982, 95.837, 95.416, 94.703, 93.657]
Unseen acc:
[69.52, 58.842, 57.101, 50.697, 49.859]
Harmonic mean:
[80.636, 72.915, 71.446, 66.041, 65.075]
```
- Why is the result so different from the base code?
```txt loss_lpc 0 without noise
[97.134, 94.595, 91.894, 88.732, 85.543, 82.135]
Seen acc:
[96.581, 96.212, 95.514, 94.799, 93.613]
Unseen acc:
[68.457, 62.243, 57.942, 53.164, 49.452]
Harmonic mean:
[80.123, 75.586, 72.128, 68.124, 64.717]
```
## 2025-03-31
```txt loss_lpc 0.5, noise_prob 0, loss_lpc 0.25
[97.488, 94.719, 91.801, 88.953, 85.91, 82.514]
Seen acc:
[96.662, 96.281, 95.608, 94.881, 93.644]
Unseen acc:
[68.99, 60.914, 58.655, 54.212, 49.93]
Harmonic mean:
[80.515, 74.619, 72.706, 69.0, 65.132]
```
## 2025-04-02
```txt tested with formant emebeddings, 10 epochs
[65.591, 60.518, 55.502, 51.349, 48.654, 45.829]
Seen acc:
[62.467, 59.595, 57.676, 56.78, 55.042]
Unseen acc:
[33.413, 27.964, 22.81, 20.492, 20.498]
Harmonic mean:
[43.538, 38.066, 32.691, 30.115, 29.872]
```
## 2025-04-04
```txt loss_lpc 1, loss_lpc_shuffled 1, 50 epochs without noise
[97.412, 94.833, 91.46, 87.904, 84.732, 81.261]
Seen acc:
[96.584, 96.314, 95.618, 94.939, 93.605]
Unseen acc:
[71.704, 58.126, 52.965, 49.108, 46.378]
Harmonic mean:
[82.305, 72.499, 68.169, 64.733, 62.025]
```
## 2025/04/05
> [!fail] This result is from wrong code.
```txt Learning with formant, loss_lpc 0, 50 epochs without noise
[84.727, 80.034, 76.791, 73.247, 70.028, 66.685]
Seen acc:
[82.527, 81.844, 80.534, 79.771, 78.767]
Unseen acc:
[47.87, 42.678, 40.614, 36.329, 33.472]
Harmonic mean:
[60.593, 56.102, 53.997, 49.922, 46.98]
```
> [!fail] This result is from wrong code.
```txt Learning with residual, loss_lpc 0, 50 epochs without noise
[91.401, 88.079, 85.172, 82.566, 79.142, 75.919]
Seen acc:
[90.56, 89.852, 89.285, 88.017, 86.56]
Unseen acc:
[54.958, 53.161, 51.952, 48.507, 45.416]
Harmonic mean:
[68.404, 66.8, 65.684, 62.545, 59.575]
```
## 2025/04/09
> [!note] Only with residual, base, 100 epochs
> [[Server#2025/04/07]]
[80.453, 75.463, 70.758, 67.637, 64.847, 61.486]
Seen acc:
[78.97, 77.367, 76.484, 75.265, 73.624]
Unseen acc:
[29.246, 26.035, 27.72, 28.915, 27.927]
Harmonic mean:
[42.684, 38.96, 40.692, 41.779, 40.494]

> [!note] Only with formant, base, 100 epochs
> [[Server#2025/04/09]]
[60.008, 57.082, 53.249, 51.045, 48.206, 45.351]
Seen acc:
[59.365, 57.648, 56.573, 55.76, 54.647] 
Unseen acc:
[27.154, 23.551, 26.167, 22.81, 19.21]
Harmonic mean:
[37.263, 33.441, 35.783, 32.376, 28.427]

> [!note] Formant and residual classification loss added, `noise_prob` 0.0
> [[Server#2025/04/09]]
> [[formant_residual_classification_loss#2025/04/09]]
[97.314, 95.027, 92.65, 90.265, 87.408, 84.126]
Seen acc:
[96.574, 96.441, 95.685, 94.525, 93.451]
Unseen acc:
[74.53, 66.572, 65.408, 61.767, 57.447]
Harmonic mean:
[84.132, 78.77, 77.701, 74.713, 71.154]

## 2025/04/10
> [!note] Formant and residual classification loss added, `noise_prob` 0.8
> [[Server#2025/04/10]]
> [[formant_residual_classification_loss#2025/04/09]]
[96.555, 94.151, 91.687, 89.469, 85.806, 82.396]
Seen acc:
[95.876, 95.607, 95.054, 92.944, 91.425]
Unseen acc:
[71.288, 64.78, 64.015, 60.337, 56.397]
Harmonic mean:
[81.774, 77.231, 76.506, 73.172, 69.761]
- Performance still drops a bit when noise is applied to the speech, formant, and residual.
- Why not applying noise only to the speech? [[formant_residual#2025/04/10]]

## 2025/04/11
- Set `lpc_loss_shuffled` to 0.
---
```python helper.py
            total_loss = (
                loss
                + loss_a_freq  # classification loss based on formant
                + loss_res  # classification loss based on residual
                + args.loss_lpc * loss_lpc
                + args.loss_lpc * loss_lpc_shuffled * 0
            )```
---
> [!note] Formant and residual classification loss added, `noise_prob` 0.0, `lpc_loss_shuffled` 0
> [[Server#2025/04/09]]
> [[formant_residual_classification_loss#2025/04/09]]
[97.275, 94.96, 92.535, 90.533, 87.26, 84.577]
Seen acc:
[96.399, 96.244, 95.691, 94.965, 93.994]
Unseen acc:
[76.034, 66.901, 66.881, 59.928, 57.748]
Harmonic mean:
[85.014, 78.934, 78.733, 73.484, 71.542]

> [!note] `loss_lpc` 0, 2 ways 5 shots, 10 incremental sessions
> [[Server#2025/04/11]]
> [[adjust_base_class#2025/04/11]]
> [[fewer_base_classes#2025/04/11]]
> [!Error]

## 2025/04/12
> [!note] Formant and residual classification loss
> - `noise_prob` 0.0
> - `lpc_loss_shuffled` 0
> - **LPC order 20**
> [!fail]  `loss_lpc` was set to 0, and only classification loss of formant and residual.
[96.997, 94.482, 92.245, 90.031, 87.343, 84.378]
Seen acc:
[96.172, 95.966, 95.568, 94.7, 93.918]
Unseen acc:
[72.475, 66.866, 64.926, 61.328, 56.563]
Harmonic mean:
[82.659, 78.816, 77.322, 74.445, 70.604]

## 2025/04/13
> [!note] Config
> - noise_prob 0.0
> - lpc_loss_shuffled 0
> - LPC order 20
> - lpc_loss 1
[97.055, 94.734, 92.275, 90.009, 86.721, 83.559]
Seen acc:
[96.245, 96.124, 95.475, 94.34, 92.912]
Unseen acc:
[75.137, 65.958, 65.134, 59.698, 56.239]
Harmonic mean:
[84.391, 78.234, 77.439, 73.124, 70.067]

## 2025/04/15
> [!note] Config
> - ==Dot similarity== between speech embeddings and LPC embeddings(loss_lpc_shuffled 0).
[97.263, 94.959, 92.607, 90.084, 86.784, 83.73]
Seen acc:
[96.494, 96.24, 95.481, 94.486, 93.292]
Unseen acc:
[74.804, 67.639, 65.419, 60.555, 56.361]
Harmonic mean:
[84.276, 79.444, 77.642, 73.808, 70.27]

> [!note] Config
> - ==Dot similarity== between speech embeddings and LPC embeddings(loss_lpc_shuffled 1).
[97.339, 94.731, 92.625, 90.195, 87.128, 84.207]
Seen acc:
[96.369, 96.126, 95.416, 94.456, 93.495]
Unseen acc:
[73.289, 68.619, 66.364, 61.574, 57.855]
Harmonic mean:
[83.259, 80.076, 78.281, 74.55, 71.479]

## 2025/04/16
- `lr_base` was set to 0.0005.
- This should be fixed to 0.005.
> [!note] Config
> Dot similarity, `lr_base` modified, `lpc_loss_shuffled` 1.
[97.951, 95.676, 93.758, 90.771, 87.479, 84.441]
Seen acc:
[97.282, 97.136, 96.642, 96.014, 94.941]
Unseen acc:
[74.709, 70.757, 64.25, 58.047, 54.727]
Harmonic mean:
[84.514, 81.874, 77.185, 72.352, 69.431]

## 2025/04/17
- `loss_a_freq` and `loss_res` lowered.
```python helper.py
            total_loss = (
                loss
                + loss_a_freq * 0.5  # classification loss based on formant
                + loss_res * 0.5  # classification loss based on residual
                + args.loss_lpc * loss_lpc
                + args.loss_lpc_shuffled * loss_lpc_shuffled
            )
```
> [!warning]`loss_lpc` and `loss_lpc_shuffled` was set to 0.
> Dot similarity used to compare `emb` and `emb_rec`.
[97.927, 95.595, 93.093, 90.728, 87.807, 85.288]
Seen acc:
[97.351, 97.158, 96.689, 95.902, 95.143]
Unseen acc:
[72.471, 65.17, 63.632, 59.49, 55.967]
Harmonic mean:
[83.088, 78.012, 76.752, 73.43, 70.477]

## 2025/04/18
> [!note] Config
> - `loss_lpc` and `loss_lpc_shuffled` set to 1, `loss_a_freq` and `loss_res` set to 0.5.
> - Dot similarity used to compare `emb` and `emb_rec`.
> - Full config : [[Server#2025/04/17]]
[97.72, 95.277, 92.986, 90.391, 87.214, 84.595]
Seen acc:
[96.694, 96.499, 95.909, 95.144, 94.235]
Unseen acc:
[76.478, 68.598, 65.155, 59.328, 55.571]
Harmonic mean:
[85.406, 80.191, 77.596, 73.084, 69.914]

## 2025/04/19
> [!note] Config
> Same setting with [[#2025/04/18]], but with no Transformer layer.
[98.024, 95.928, 93.718, 91.254, 88.062, 85.054]
Seen acc:
[97.08, 96.936, 96.265, 95.354, 94.329]
Unseen acc:
[80.894, 71.596, 68.468, 62.431, 57.088]
Harmonic mean:
[88.251, 82.361, 80.021, 75.458, 71.129]

> [!note]
> Exactly the same with above, but with 2d embeddings extraction.
> [!warning]
> There was an error about [[2d_embeddings_with_lpc_code#2025/04/20]]
[98.085, 95.695, 93.853, 91.44, 88.431, 85.874]
Seen acc:
[97.363, 97.228, 96.697, 96.094, 95.384]
Unseen acc:
[73.484, 70.522, 67.533, 61.882, 56.856]
Harmonic mean:
[83.755, 81.749, 79.526, 75.283, 71.245]
- I thought the result should be exactly the same with the previous experiment, but somehow it's differnet,
  especially in 1st incremental session.

## 2025/04/20
> [!note]
> Exactly the same with [[#2025/04/19]], but with 2d embeddings of LPC related features extracted.
[98.085, 95.695, 93.853, 91.44, 88.431, 85.874]
Seen acc:
[97.363, 97.228, 96.697, 96.094, 95.384]
Unseen acc:
[73.484, 70.522, 67.533, 61.882, 56.856]
Harmonic mean:
[83.755, 81.749, 79.526, 75.283, 71.245]

## 2025/04/21
> [!note]
> Exactly the same with [[#2025/04/20]], with `loss_lpc` and `loss_lpc_shuffled` set to 5.
> Server : [[Server#2025/04/21]]
[96.994, 94.458, 92.585, 90.354, 87.002, 83.234]
Seen acc:
[95.883, 95.385, 94.698, 93.987, 92.91]
Unseen acc:
[75.701, 73.463, 70.867, 62.007, 55.459]
Harmonic mean:
[84.605, 83.001, 81.067, 74.719, 69.458]

> [!note]
> Exactly the same with the experiment above, with `lr_base` set to 0.002.
> [[Server#2025/04/21]]
[97.473, 94.686, 92.449, 89.559, 86.259, 83.36]
Seen acc:
[96.262, 95.813, 95.187, 94.71, 93.88]
Unseen acc:
[73.906, 69.516, 64.168, 56.61, 52.248]
Harmonic mean:
[83.615, 80.573, 76.659, 70.864, 67.134]

> [!note]
> Set `loss_lpc` and `loss_lpc_shuffled` to 0.
> [[Server#2025/04/21]]
[97.817, 95.47, 93.346, 91.142, 88.757, 85.838]
Seen acc:
[97.328, 97.217, 96.773, 96.207, 95.074]
Unseen acc:
[71.239, 66.857, 65.554, 62.306, 57.614]
Harmonic mean:
[82.265, 79.228, 78.161, 75.631, 71.749]
- The model is confused a bit with ==down== in the base session and ==wow== in the incremental session.

## 2025/04/22
> [!note]
> LPC augmentation applied, same setting with the last result in [[#Experiments-2025/04/21]].
> Server : [[Server#2025/04/22]]
> [!fail]  Stopped. `loss_lpc` and `loss_lpc_shuffled` was set to 1.

## 2025/04/23
> [!note]
> Residual augmentation, `loss_lpc` 0.
> Server : [[Server#2025/04/23]]
[97.171, 95.107, 91.952, 89.029, 85.988, 83.045]
Seen acc:
[96.691, 96.594, 96.019, 95.7, 94.388]
Unseen acc:
[74.194, 60.171, 57.262, 52.053, 48.508]
Harmonic mean:
[83.962, 74.151, 71.741, 67.43, 64.083]

> [!note]
> Base with current code.
> `loss_lpc` and `loss_lpc_shuffled` set to 0, LPC classification loss set to 0.
> Server : [[Server#2025/04/23]]
[97.817, 95.47, 93.346, 91.142, 88.757, 85.838]
Seen acc:
[97.328, 97.217, 96.773, 96.207, 95.074]
Unseen acc:
[71.239, 66.857, 65.554, 62.306, 57.614]
Harmonic mean:
[82.265, 79.228, 78.161, 75.631, 71.749]
- Idea : [[confusing_keywords#2025/04/23]]

## 2025/04/24
> [!note]
> Both formant and residual augmentation.
> Server : The last experiment in [[Server#2025/04/23]]
[97.653, 95.265, 92.827, 90.198, 86.83, 84.009]
Seen acc:
[96.951, 96.902, 96.242, 95.69, 94.304]
Unseen acc:
[73.11, 64.732, 62.617, 56.005, 53.047]
Harmonic mean:
[83.359, 77.616, 75.871, 70.656, 67.9]

> [!note]
> Residual augmentation only.
> Server : The last experiment in [[Server#2025/04/23]]
[97.022, 94.13, 91.981, 89.413, 86.533, 83.405]
Seen acc:
[96.245, 96.075, 95.619, 94.871, 93.697]
Unseen acc:
[66.71, 64.217, 61.421, 57.482, 53.624]
Harmonic mean:
[78.801, 76.98, 74.796, 71.589, 68.21]

> [!note]
> Formant augmentation only.
> Server : The last experiment in [[Server#2025/04/23]]
[97.619, 95.301, 92.565, 89.762, 85.917, 83.135]
Seen acc:
[97.129, 96.896, 96.474, 95.736, 94.849]
Unseen acc:
[71.142, 62.791, 59.318, 52.169, 49.402]
Harmonic mean:
[82.129, 76.202, 73.465, 67.536, 64.966]

> [!note]
> The experiments above are done with wrong code, [[code_refactoring#2025/04/24]].
> Server : [[Server#2025/04/24]]
[97.97, 94.855, 91.414, 88.007, 84.536, 81.477]
Seen acc:
[97.359, 97.262, 96.926, 96.088, 95.289]
Unseen acc:
[61.938, 51.388, 47.561, 44.365, 41.264]
Harmonic mean:
[75.71, 67.247, 63.811, 60.703, 57.589]

## 2025/04/30
> [!note]
> Base method with the new code (50 epochs).
> Server : [[Server#2025/04/30]]
[96.62, 94.175, 91.23, 88.869, 85.13, 82.342]
Seen acc:
[96.084, 95.788, 95.198, 94.131, 92.889]
Unseen acc:
[69.283, 60.005, 60.064, 53.924, 50.154]
Harmonic mean:
[80.512, 73.787, 73.656, 68.568, 65.138]

## 2025/05/01
> [!note]
> Base method with the new code (100 epochs).
> Server : [[Server#2025/05/01]]
[97.031, 94.585, 91.471, 88.698, 84.902, 81.441]
Seen acc:
[96.456, 96.297, 95.877, 94.842, 93.527]
Unseen acc:
[70.204, 58.123, 56.025, 49.901, 47.502]
Harmonic mean:
[81.262, 72.492, 70.723, 65.395, 63.004]

> [!note]
> set `loss_a_freq_cls`, `loss_res_cls` to 1.
> Server : [[Server#2025/05/01]]
[97.305, 95.39, 93.431, 91.081, 88.428, 85.541]
Seen acc:
[96.852, 96.608, 95.815, 94.929, 93.921]
Unseen acc:
[76.153, 71.779, 69.695, 65.563, 60.383]
Harmonic mean:
[85.264, 82.363, 80.694, 77.559, 73.507]
- result is pretty good.. Should test the comp code and compare with the result above.

> [!note]
> Experiment with `comp` code(`noise_prob` 0.0).
> Server : [[Server#2025/05/01]]
[98.561, 96.768, 94.883, 92.937, 90.109, 87.423]
Seen acc:
[98.17, 98.123, 97.822, 97.06, 96.0]
Unseen acc:
[78.427, 72.707, 70.674, 65.959, 61.092]
Harmonic mean:
[87.195, 83.524, 82.061, 78.543, 74.667]

## 2025/05/02
> [!note]
> Experiment with `comp` code(`noise_prob` 0.8).
> Server : [[Server#2025/05/02]]
[97.305, 95.39, 93.431, 91.081, 88.428, 85.541]
Seen acc:
[96.852, 96.608, 95.815, 94.929, 93.921]
Unseen acc:
[76.153, 71.779, 69.695, 65.563, 60.383]
Harmonic mean:
[85.264, 82.363, 80.694, 77.559, 73.507]
- Result with no noise is better.
