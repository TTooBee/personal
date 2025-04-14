---
id: load_from_file_nan
aliases: []
tags: []
---

# load from file nan

## 2025-03-27
- If loaded from files, all the embeddings are nan. Think it means there might be a problem with dtype?
  - No problem with this maybe.. The value itself saved in pt files doesn't seem that wrong.
- **Found what was wrong!** There was nan in res_spec and a_freq.
