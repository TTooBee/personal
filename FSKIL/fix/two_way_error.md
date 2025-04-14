---
id: two_way_error
aliases: []
tags: []
---

# two_way_error

## 2025/04/12
- Print `new_fc.shape` to for debugging.
- Shape of `new_fc` is [3, 512]. This means `arsg.way` is not applied in to `self.update_fc_avg`.
  - One way to ignore this is just to maintain the size of ways to 3, and adjust the length of the
    incremental sessions, but let's try fixing the code first.
- Check `class_list` and `len(class_list)`.
  - `len(classlist)` is 3, and there are three classes in `class_list`. Let's go to the part that 
    `class_list` is from.
- `class_list` is from `np.unique(train_set.target)`. Think the problem is from dataloader.
  - Let's go to `get_dataloader` in `data_utils.py` and check how the target is made.

- The code below is where trainset is made, and the set is get from the txt file.
  - The audio files to be used for each session are **Fixed** so changing the size of the way and sessions
    doesn't work.
  - Maybe I can look at the data preparation code.
---
```python data_utils.py get_new_dataloader
def get_new_dataloader(args, session):
    txt_path = (
        "data/index_list/" + args.dataset + "/session_" + str(session + 1) + ".txt"
    )

    if args.dataset == "gsc2":
        trainset = args.Dataset.GSC2(
            root=args.dataroot, args=args, train=True, index_path=txt_path
        )```
---

- This is the code where the txt file is made.
```python dataset_prep.py MakeIndex
    for session in range(2, 7):
        start_folder = 21 + (session - 2) * 3
        end_folder = start_folder + 2
        session_paths = []```
