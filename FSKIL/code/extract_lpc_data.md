---
id: extract_lpc_data
aliases: []
tags: []
---

# extract_lpc_data

## 2025/04/11
> [!note]
> The code that extracts LPC related features : "~/Projects/FSKIL/practice/data.py"
---
```python data.py
    def __call__(self, x, noise_prob=0.8, is_train=True, base_sess=False):
        x = self._pad_audio(x)
        x = self._apply_augmentation(x, noise_prob)

        a_all, a_freq_all, res = self.get_lp_residu(x, 10, 160) # lpc order : 10```
---
- Set `lpc_order` to 10 and run the code.

## 2025/05/17
