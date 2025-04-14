---
id: save_embeddings
aliases: []
tags: []
---

# save_embeddings

## 2025/04/11
--- [!note]
> Find the parts to modify(or remove, uncomment) with the query `save embeddings`.
---
- Three parts to add.
  1. Make empty lists for embeddings.
  ---
  ```python helper.py
        # save embeddings
        if session == 5:
          emb_list = []
          emb_a_freq_list = []
          emb_res_list = []
          test_label_list = []```
  ---

  2. Get embeddings from the model.
  ---
  ```python helper.py
                    # save embeddings
                    if session == 5:
                      emb, _ = model.module.encode(data)
                      emb_a_freq, _ = model.module.encode(a_freq_data)
                      emb_res, _ = model.module.encode(res_data)

                      emb_list.append(emb)
                      emb_a_freq_list.append(emb_a_freq)
                      emb_res_list.append(emb_res)
                      test_label_list.append(test_label)```
  ---

  3. Save the tensor to .pt files.
  ---
  ```python helper.py
        # save embeddings
        if session == 5:
          emb_tensor = torch.cat(emb_list, dim=0)
          emb_a_freq_tensor = torch.cat(emb_a_freq_list, dim=0)
          emb_res_tensor = torch.cat(emb_res_list, dim=0)
          test_label_tensor = torch.cat(test_label_list, dim=0)
          torch.save(
              test_label_tensor,
              "/home/sehyun/Projects/FSKIL/practice/test_label.pt",
          )
          torch.save(emb_tensor, "/home/sehyun/Projects/FSKIL/practice/emb.pt")
          torch.save(
              emb_a_freq_tensor,
              "/home/sehyun/Projects/FSKIL/practice/emb_a_freq.pt",)
          torch.save(emb_res_tensor, "/home/sehyun/Projects/FSKIL/practice/emb_res.pt")```
  ---
- Uncomment these parts before you run the model with 0 epochs with a pre-train model.
- Use the base model for the pre-trained model.
  - But the name of the parameters would be different if I just use the model trained in base projects,
    then should I train the model again with `lpc_loss` 0...?
    - Let's do this at night.
