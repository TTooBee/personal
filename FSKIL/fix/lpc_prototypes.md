---
id: lpc_prototypes
aliases: []
tags: []
---

# lpc_prototypes

## 2025/04/17
- Below is the part gets the embeddings of speech, formant, and residual.
```python helper.py replace_fc
    def update_fc(self, dataloader, class_list, session):
        for batch in dataloader:
            if self.args.load_from_file == 1:
                data_ori, res_data, a, a_freq_data, label = [_.cuda() for _ in batch]
                res_data = torch.nan_to_num(res_data, nan=0.0, posinf=0, neginf=0)
                a_freq_data = torch.nan_to_num(a_freq_data, nan=0.0, posinf=0, neginf=0)
```
1. Below is the part that gets the `new_fc` by averaging the embeddings.
```python replace_fc
        else:
            new_fc = self.update_fc_avg(data, label, class_list)
            new_fc_map = self.update_fc_map_avg(data2, label, class_list)
```
2. Below is the part that replaces the prototypes to the average of the embeddings.
```python replace_fc
        self.fc.weight.data[
            self.args.base_class + (session - 1) * self.args.way : self.args.base_class
            + session * self.args.way
        ] = new_fc
```
