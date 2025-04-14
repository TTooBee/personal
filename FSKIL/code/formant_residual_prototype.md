---
id: format_residual_prototype
aliases: []
tags: []
---

## 2025-03-31
- `model.module.fc` is defined in `Network.py`.

---
```python Network.py model.fc
class MYNET(nn.Module):
    def __init__(self, args, mode=None):
        super().__init__()

        self.mode = mode
        self.args = args

        if self.args.dataset in ["gsc2"]:
            self.encoder = bcresnet8(args, args.tau, args.num_classes)
            if not args.sequential_layer == "None":
                self.num_features = 32 * args.tau * 2  # doubled
            else:
                self.num_features = 32 * args.tau
                # self.num_features = (
                #     32 * args.tau * 2
                # )  # temporarily set num_features to 512, should be removed
            self.num_features_pri = 32 * args.tau
            # self.num_features_pri = 32 * args.tau * 2  # this line should be removed

        self.pre_allocate = self.args.num_classes
        self.fc = nn.Linear(self.num_features, self.args.num_classes, bias=False)
```
---

- Should make a parameter in the same way, named `self.fc_formant`, `self.fc_residual`.

---
```python Network.py formant residual prototype fc added
        self.fc = nn.Linear(self.num_features, self.args.num_classes, bias=False)
        self.fc_formant = nn.Linear(
            self.num_features, self.args.num_classes, bias=False
        )
        self.fc_residual = nn.Linear(
            self.num_features, self.args.num_classes, bias=False
        )
```
---

- Should modify fc updating part in `Network.py`.

---
```python helper.py replace_base_fc when args.vector_based_only == 1
    else:
        embedding_list = []
        label_list = []
        with torch.no_grad():
            for i, batch in enumerate(trainloader):
                if args.load_from_file == 1:
                    data, res_data, a, a_freq_data, label = [_.cuda() for _ in batch]
                else:
                    data, label = [_.cuda() for _ in batch]
                model.module.mode = "encoder"
                embedding, embedding2 = model(data)

                embedding_list.append(embedding.cpu())
                label_list.append(label.cpu())
        embedding_list = torch.cat(embedding_list, dim=0)
        label_list = torch.cat(label_list, dim=0)

        proto_list = []

        for class_index in range(args.base_class):
            data_index = (label_list == class_index).nonzero()
            embedding_this = embedding_list[data_index.squeeze(-1)]
            embedding_this = embedding_this.mean(0)
            proto_list.append(embedding_this)

        proto_list = torch.stack(proto_list, dim=0)

        model.module.fc.weight.data[: args.base_class] = proto_list

    return model
```
---

- Think everything is done for making the prototypes for formant and residual.
- One more thing to do is to make the model actually learn by the formant and residual embeddings.
- `forward_dummy` defined in `Network.py`, and used in `helper.py` can be used for this.

---
```python helper.py forward_dummy being used
        emb_aug, dummy_classifier = model.module.forward_dummy(data_aug)
```
---

- The outputs of `forward_dummy` are `emb_aug` and `dummy_classifier` which are the output of the masked spectrum and the output of the feature generator.
- `dummy_classifier` can be made by the prototype mixup with the prototypes of formant and the residual inside the batch.

---
```python Network.py CompExtractor
class CompExtractor(nn.Module):
    def __init__(self, z_dim, L):
        super(CompExtractor, self).__init__()
        """
        DeepSets Function
        """
        self.L = L
        self.z_dim = z_dim

        self.hidden = 64  # z_dim*4
        self.gen1 = nn.Linear(z_dim, self.hidden)
        self.gen2 = nn.Linear(self.hidden, z_dim, bias=False)

        self.norm1 = nn.BatchNorm1d(self.hidden)
        self.norm2 = nn.BatchNorm1d(self.z_dim)

    def forward(self, set_input):
        """
        set_input, seq_length, set_size, dim
        """
        num_proto, dim = set_input.shape

        h = self.norm1(self.gen1(set_input))
        h = F.relu(h)

        combined_mean = self.norm2(self.gen2(h))
        combined_mean = F.relu(combined_mean)

        return combined_mean  # compositional information
```
---

- Or maybe add a class about Manifold Mixup.
- The input of the `self.set_func` which is the function made by the class `CompExtractor` is 
  `self.fc.weight[: self.args.base_class]`.
- The inputs of `forward_dummy` are `data`, `res_data`, and `a_freq_data`, and the inputs of `self.set_func` are
  `self.fc_formant.weight`, `self.fc_residual.wegiht`.
> [!note] `update_fc` runs only in incremental sessions.
---
```python Network.py CompExtractor
  def forward(self, set_input):
        """
        set_input, seq_length, set_size, dim
        """
        num_proto, dim = set_input.shape

        h = self.norm1(self.gen1(set_input))
        h = F.relu(h)

        combined_mean = self.norm2(self.gen2(h))
        combined_mean = F.relu(combined_mean)

        return combined_mean  # compositional information
```
---
- `set_input` is `self.fc.weight`. This should be modified to get the prototypes of the formant and the residual.
