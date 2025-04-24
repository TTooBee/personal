---
id: 2d_embeddings_with_lpc_code
aliases: []
tags: []
---

# 2d_embeddings_with_lpc_code

## 2025/04/19
- The code below is the part that gets the embeddings during the base training session.
---
> [!tip]
> Don't need to do something related to algorithm. Just make it able to extract the 2d embeddings
> of the LPC related features.
> Parts to add:
> 1. `replace_base_fc`
> 2. `update_fc`
---
> [!note]
> There were some problems but all solved now...
---

## 2025/04/20
---
> [!fail]
> `fc_map` and `fc_ma[_residual` is exactly the same.
---
- They're the same at the last part(right before return).
```python helper.py replace_base_fc(test code)
    fc_map = model.module.fc_map.data[: args.base_class]
    fc_map_formant = model.module.fc_map_formant.data[: args.base_class]
    fc_map_residual = model.module.fc_map_residual.data[: args.base_class]
    target = 18
    print(f"speech : {torch.norm(fc_map[target, :, :], p=2, dim=0)}")
    print(f"formant : {torch.norm(fc_map_formant[target, :, :], p=2, dim=0)}")
    print(f"residual : {torch.norm(fc_map_residual[target, :, :], p=2, dim=0)}")

    return model
```
- [!success] Solved!
```python Network.py __init__.py
        self.fc_map = nn.Parameter(
            torch.rand(args.num_classes, self.num_features_pri, args.num_features_pri)
        )
        self.register_parameter("fc_map", self.fc_map)

        self.fc_map_formant = nn.Parameter(
            torch.rand(args.num_classes, self.num_features_pri, args.num_features_pri)
        )
        self.register_parameter("fc_map", self.fc_map_formant)

        self.fc_map_residual = nn.Parameter(
            torch.rand(args.num_classes, self.num_features_pri, args.num_features_pri)
        )
        self.register_parameter("fc_map", self.fc_map_residual)
```
- These three different variables were sharing the id which means that they were sharing the memory.
