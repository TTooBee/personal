---
id: dot_similarity
aliases: []
tags: []
---

# dot_similarity

## 2025/04/15
- Use dot similarity between `emb_rec` and `emb` instead of cosine similarity.
- Sigmoid is used to limit the similarity value smaller than 1.
---
> [!warning]
> *The code below should be changed. It's a wrong code.*
> I wasn't multiplying the output of the sigmoid to the *normalized* embeddings.
---
- Should put in the size of the embeddings to the sigmoid function, and multiply this to the normalized vector.

```python helper.py base_train
            elif args.emb_rec_similarity == "dot":
                # torch.norm(emb, p=2, dim=1) : size of the vector
                norm_emb = torch.norm(emb, p=2, dim=1)
                emb_scale = F.sigmoid(
                    (norm_emb - torch.min(norm_emb))
                    / (torch.max(norm_emb) - torch.min(norm_emb))
                )
                norm_emb_rec = torch.norm(emb_rec, p=2, dim=1)
                emb_rec_scale = F.sigmoid(
                    (norm_emb_rec - torch.min(norm_emb_rec))
                    / (torch.max(norm_emb_rec) - torch.min(norm_emb_rec))
                )

                cos_sim_lpc = torch.sum(emb * emb_rec, dim=1) / (
                    torch.norm(emb, dim=1) * torch.norm(emb_rec, dim=1)
                )
                cos_sim_lpc_shuffled = torch.sum(emb * emb_rec_shuffled, dim=1) / (
                    torch.norm(emb, dim=1) * torch.norm(emb_rec_shuffled, dim=1)
                )
```
- Made this to a function.
```python helper.py get_dot_sim_loss
def get_dot_sim_loss(emb, emb_rec):
    norm_emb = torch.norm(emb, p=2, dim=1)
    emb_scale = F.sigmoid(
        (norm_emb - torch.min(norm_emb)) / (torch.max(norm_emb) - torch.min(norm_emb))
    )
    norm_emb_rec = torch.norm(emb_rec, p=2, dim=1)
    emb_rec_scale = F.sigmoid(
        (norm_emb_rec - torch.min(norm_emb_rec))
        / (torch.max(norm_emb_rec) - torch.min(norm_emb_rec))
    )

    cos_sim_lpc = torch.sum(emb * emb_rec, dim=1) / (
        torch.norm(emb, dim=1) * torch.norm(emb_rec, dim=1)
    )

    loss_lpc = 1 - torch.mean(emb_scale * cos_sim_lpc)

    return loss_lpc
```
---
> [!note]
> The code standardizes the size(L2 norm) of the vectors in the range 0 to 1 with ==min-max== value.
> Try to change this to standardize the size of the vectors with ==mean== and ==std==.
> TODO : [[April#Tomorrow-2025/04/15]]
---
```python helper.py base_train
            elif args.emb_rec_similarity == "dot":
                loss_lpc = get_dot_sim_loss(emb, emb_rec)
                loss_lpc_shuffled = get_dot_sim_loss(emb, emb_rec_shuffled)
```

- Activate the dot similarity in the training setting script.
```sh gsc2.sh
python train.py trans \
  ... \
  -emb_rec_similarity "dot"
```
