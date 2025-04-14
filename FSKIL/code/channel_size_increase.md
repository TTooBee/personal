---
id: channel_size_increase
aliases: []
tags: []
---

# channel_size_increase

## 2025-03-19
- Should modify `bcresnet.py`.
- Think it's related to `tau`, and it's the hyper-parameter that I can manage. It's set to 8 and it makes the final embedding size 256, and if it's set to 16, maybe the embedding size will be doubled.
  - Yes it does happen.

## 2025-03-20
- Set `model.num_features` to 512.

---

```python Network.py
        if self.args.dataset in ["gsc2"]:
            self.encoder = bcresnet8(args, args.tau, args.num_classes)
            if not args.sequential_layer == "None":
                self.num_features = 32 * args.tau * 2  # doubled
            else:
                # self.num_features = 32 * args.tau
                self.num_features = 32 * args.tau * 2 # temporarily set num_features to 512
          # self.num_features_pri = 32 * args.tau
            self.num_features_pri = 32 * args.tau * 2 # this line should be removed
```
---

- Set ==only the last embedding size== to 512.

---
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
            - [ ] int(base_c * 2.5),
            # base_c * 4,
            base_c * 8,
        ]  # num channels
        self.s = [1, 2]  # stage using stride
        self._build_network()
```
---

- Only the last embedding size got increased. [[Experiments#2025-03-20]]
