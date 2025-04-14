---
id: adjust_base_class
aliases: []
tags: []
---

# adjust_base_class

## 2025/04/11
- The code below is the part related to the size of the base classes(and the others).
___
```python data_utils.py
def set_up_datasets(args):
    if args.dataset == "gsc2":
        if args.project == "primitive":
            import dataloader.gsc2.gsc2_prim as Dataset
        else:
            import dataloader.gsc2.gsc2 as Dataset

        args.base_class = 15
        args.num_classes = 35
        args.way = 2
        args.shot = 5
        args.sessions = 11

    args.Dataset = Dataset
    return args```
---
> [!note] Keywords
> [[confusing_keywords#keywords]]]
---
- "four" and "three" are the keywords the model is most confused about, and I'm targeting to fix this problem,
  but there 18th and 19th, so it's could be a bit dangerous if I train the model with just decreased base class size.
- 2 ways 5 shots, 10 incremental sessions.
---
> [!error]
> Error occurred while replacing the `model.fc`.
> Fix [[two_way_error#2025/04/12]]
---
