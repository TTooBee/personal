---
id: freeze_parameter
aliases: []
tags: []
---

# freeze_parameter

## 2025/05/14
- I should modify the parameter itself (and the optimizer) to freeze the parameter.
- The code below is the part that initializes the parameter.
```python Network.py __init__
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
        self.fc_formant = nn.Linear(
            self.num_features, self.args.num_classes, bias=False
        )
        self.fc_residual = nn.Linear(
            self.num_features, self.args.num_classes, bias=False
        )
```
- Should make the gradients of the parameter none like the code below.
```python Network.py __init__
        # freeze the residual prototype
        if epoch > args.epoch_freeze_classifier:
            for param in model.fc_residual.parameters():
                param.requires_grad = False

        optimizer.zero_grad()
        total_loss.backward()
        optimizer.step()
```
