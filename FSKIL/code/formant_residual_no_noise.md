---
id: formant_residual_no_noise
aliases: []
tags: []
---

# formant_residual_no_noise

## 2025/04/10
- Have to look at the part that gets the noisy formant and residual.
  - The code below is the part that loads the pt file of the speech spectrum, formant, and residual.
  - It's modified to load **clean** formant and residual regardless of `args.noise_prob`.
---
```python gsc2.py load_pt_file
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
            f"noise_prob_0.0",
            "keywords_residual",
            keyword,
            pt_file_name,
        )
        a_pt_file_path = os.path.join(
            self.root,
            "GSC2",
            f"noise_prob_0.0",
            "keywords_a",
            keyword,
            pt_file_name,
        )
        a_freq_pt_file_path = os.path.join(
            self.root,
            "GSC2",
            f"noise_prob_0.0",
            "keywords_a_freq",
            keyword,
            pt_file_name,
        )```
---
