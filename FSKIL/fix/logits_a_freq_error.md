---
id: logits_a_freq_error
aliases: []
tags: []
---

# logits a_freq error

## 2025/04/07

### formant_error-2025/04/07
- The error message is below.
---
[!error]
```log logits a_freq error
File "/home/sehyun/Projects/FSKIL/models/trans/helper.py", line 455, in test
    lgt = lgt.view(-1, test_class)
          ^^^^^^^^^^^^^^^^^^^^^^^
RuntimeError: shape '[-1, 20]' is invalid for input of size 286335
```
---

- Think it's because of the part below
  - `logits` were being sliced, but `logits_a_freq` and `logits_res` were not.

---
```python helper.py
            logits = logits[:, :test_class]
            logits_a_freq = logits_a_freq[:, :test_class]
            logits_res = logits_res[:, :test_class]
```
---

- If solved, then I should add this part to [[lpc#2025/04/05]].
- **SOLVED!**
- But another problem occurred. Nan test loss and low test accuracy.
- Logits itself do not have any NaN.
- There are several NaNs in loss.

### residual_error-2025/04/07
- Residual only embedding based model performance gets terrible in incremental session.
- Think this is because there is something wrong with replacing the `fc.weight` in
incremental session. *Maybe not..?*
- When being tested with a two total different domain data(like trained with speech and
  tested with residual), then this kind of thing happens.
  - Would there be some other parts than [[lpc#2025/04/05]] to modify?

## 2025/04/08
- Test after a single epoch works well, so it seems like there is a problem is the incremental session.
---
> [!example] Candidates for the problem
> - NaN data..?
> - There is a normalizing part of the vector, and there will be nan if the size of the vector is 0.
---
- There should be nan value in logits_res. If not, there can't be nan after the cross entropy block.
  - And yes there are a lot of nan values in logits_res.
  - There were no nan value inside `forward_metric`, when look at the embeddings, 
    so the cosine similarity value must be the problem.

---
```python Network.py forward_metric
    def forward_metric(self, x):
        x, x_2d = self.encode(x)
        x_2d = torch.squeeze(x_2d, dim=2)
        emb = x

        x = F.linear(
            F.normalize(x, p=2, dim=-1), F.normalize(self.fc.weight, p=2, dim=-1)
        )
        if torch.isnan(x).any():
            print("########################")
            print(F.normalize(self.fc.weight, p=2, dim=-1))
            print("########################")
```
---

- NaN appears here. Think we should try without normalization(use dot similarity).
- **Think it's solved.**
---

> [!success] Solved!
> All the parts that load formant or residual should have the code that changes nan values to a number
> right after there loaded.
---

---
```python nan_to_num
            res_data = torch.nan_to_num(res_data, nan=0.0, posinf=0, neginf=0)
            a_freq_data = torch.nan_to_num(a_freq_data, nan=0.0, posinf=0, neginf=0)
```
---
