---
id: transformer_module
aliases: []
tags: []
---

# transformer_module

## 2025-03-14

- Should deleted all the parts related to logits2, and make logits doubled sized.
- `fc.weight` also should be doubled.
- Network.py : `self.num_features = 32 * args.tau * 2  # doubled`. Modified the size of the prototype since it'll be the concatenated vector of emb_cnn and emb_transformer.
- Maybe just need to make x to the concatenated vector and change nothing else?

## 2025-03-17

- If `args.use_transformer_layers == 1`, channel size of BC-ResNet : 512, else : 256.

``` python bcresnet.py
    def forward(self, x):  # [B, 1, 40, 101]
        x = self.cnn_head(x)
        for i, num_modules in enumerate(self.n):
            for j in range(num_modules):
                x = self.BCBlocks[i][j](x)
        x = self.classifier(x)  #  // input shape : [B, 160, 5, 101]

        x_ori = x

        if self.args.use_transformer_layers == 1:
            # self_attention useing a transformer encoder layer
            # shape of x : (batch, 256, 1, 101)
            x = self.get_diff_vectors(x)
            x = torch.squeeze(x, dim=2)
            # shape of x : (batch, 256, 101)
            x = torch.permute(x, (2, 0, 1))
            # shape of x : (101(seq), batch, 256(d_model))
            x = self.positional_encoding(x)
            x = self.self_attention(x)
            x = self.self_attention(x)
            x = torch.permute(x, (1, 2, 0))
            x = torch.unsqueeze(x, dim=2)
            # shape of x : (batch, 256, 1, 101)
            x_trans = x
            x = torch.cat((self.classifier_2(x_ori), self.classifier_2(x_trans)), dim=1)

        x = x.view(-1, x.shape[1])
        return x, x_ori
```

``` python Network.py
class MYNET(nn.Module):
    def __init__(self, args, mode=None):
        super().__init__()

        self.mode = mode
        self.args = args

        if self.args.dataset in ["gsc2"]:
            self.encoder = bcresnet8(args, args.tau, args.num_classes)
            if args.use_transformer_layers == 1:
                self.num_features = 32 * args.tau * 2  # doubled
            else:
                self.num_features = 32 * args.tau
```

[[experiments#Test Result-20250317]]

## 2025-03-18
- Should fix `train.py`, `Network.py`, `bcresnet.py`
