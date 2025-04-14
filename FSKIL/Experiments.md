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
```txt with noise
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
[59.365, 57.648, 56.573, 55.76, 54.647] Unseen acc:
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
> [!note]
> - noise_prob 0.0
> - lpc_loss_shuffled 0
> - LPC order 20
> - lpc_loss 1
[97.055, 94.734, 92.275, 90.009, 86.721, 83.559] Seen acc:
[96.245, 96.124, 95.475, 94.34, 92.912]
Unseen acc:
[75.137, 65.958, 65.134, 59.698, 56.239]
Harmonic mean:
[84.391, 78.234, 77.439, 73.124, 70.067]
