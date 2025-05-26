---
id: insert_incremental_session
aliases: []
tags: []
---

# insert_incremental_session

## 2025/05/16
- The code below is the base train code.
  - All the loop gets over in `base_train`.
```python fscil_trainer.py train
            if session == 0:  # load base class train img label
                if not args.only_do_incre:
                    logging.info(
                        f"new classes for this session:{np.unique(train_set.targets)}"
                    )
                    optimizer, scheduler = get_optimizer(args, self.model)
                    for epoch in range(args.epochs_base):
                        start_time = time.time()

                        tl, ta = base_train(
                            self.model, trainloader, optimizer, scheduler, epoch, args
                        )
                        tsl, tsa, _ = test(
                            self.model,
                            testloader,
                            epoch,
                            args,
                            session,
                            result_list=result_list,
                        )
```
- Think incremental test can be right after `test`.
- There are 00 kinds of accuracy:
  ### Things to add
  1. Seen classes'(base classes) accuracy.
  2. Unseen classes'(each session) accuracy.
  3. Unseen classes overall accuracy.
> [!question]
> What should I do with `replace_base_fc`?
> - Don't think I need this.
- The code below is the incremental session.
```python fscil_trainer.py train
            # incremental learning sessions
            else:
                logging.info("training session: [%d]" % session)
                self.model.module.mode = self.args.new_mode
                self.model.eval()
                # trainloader.dataset.transform = testloader.dataset.transform

                self.model.module.update_fc(
                    trainloader, np.unique(train_set.targets), session
                )
                # if self.args.calibration:
                #     self.model.module.soft_calibration(args, session)

                tsl, (seenac, unseenac, avgac) = test(
                    self.model,
                    testloader,
                    0,
                    args,
                    session,
                    result_list=result_list,
                )
```
- It's done only a single time in a single session, so I need to make a loop to test incremental classes
  during the base session.
- Think all I need to do is to apply that whole process right after the base test.
  - What about making it a function?
    > [!success]
- The code below is the part that gets the data.
```python fscil_trainer.py train.py
        for session in range(args.start_session, args.sessions):
            train_set, trainloader, testloader = get_dataloader(args, session)
```
- It controls the dataset and the dataloader with session.
- Should make a temporary dataset and dataloader with a temporary session inside the `base_session`(like below).
```python fscil_trainer.py base_session
            for epoch in range(args.epochs_base):
                start_time = time.time()

                tl, ta = base_train(
                    self.model, trainloader, optimizer, scheduler, epoch, args
                )
                tsl, tsa, _ = test(
                    self.model,
                    testloader,
                    epoch,
                    args,
                    session,
                    result_list=result_list,
                )
                result_list_for_inc_test = [args]
                for session_for_inc_test in range(args.start_session, args.sessions):
                    (
                        train_set_for_inc_test,
                        trainloader_for_inc_test,
                        testloader_for_inc_test,
                    ) = get_dataloader(args, session_for_inc_test)
                    result_list_for_inc_test = self.incremental_session(
                        args,
                        session_for_inc_test,
                        train_set_for_inc_test,
                        trainloader_for_inc_test,
                        testloader_for_inc_test,
                        result_list_for_inc_test,
                    )
```
- The existing code saves the better model like the code below.
```python fscil_trainer.py base_session
                # save better model
                if (tsa * 100) >= self.trlog["max_acc"][session]:
                    self.trlog["max_acc"][session] = float("%.3f" % (tsa * 100))
                    self.trlog["max_acc_epoch"] = epoch
                    save_model_dir = os.path.join(
                        args.save_path,
                        "session" + str(session) + "_max_acc.pth",
                    )
                    torch.save(dict(params=self.model.state_dict()), save_model_dir)
                    torch.save(
                        optimizer.state_dict(),
                        os.path.join(args.save_path, "optimizer_best.pth"),
                    )
                    self.best_model_dict = deepcopy(self.model.state_dict())
                    logging.info("********A better model is found!!**********")
                    logging.info("Saving model to :%s" % save_model_dir)
                logging.info(
                    "best epoch {}, best test acc={:.3f}".format(
                        self.trlog["max_acc_epoch"],
                        self.trlog["max_acc"][session],
                    )
                )
```
- The code below is where `Trainer` class is defined.
```python base.py Trainer
class Trainer(object, metaclass=abc.ABCMeta):
    def __init__(self, args):
        self.args = args
        self.args = set_up_datasets(self.args)
        self.dt, self.ft = Averager(), Averager()
        self.bt, self.ot = Averager(), Averager()
        self.timer = Timer()
        self.init_log()

    @abc.abstractmethod
    def train(self):
        pass

    def init_log(self):
        # train statistics
        self.trlog = {}
        self.trlog["train_loss"] = []
        self.trlog["val_loss"] = []
        self.trlog["test_loss"] = []
        self.trlog["train_acc"] = []
        self.trlog["val_acc"] = []
        self.trlog["test_acc"] = []
        self.trlog["max_acc_epoch"] = 0
        self.trlog["max_acc"] = [0.0] * self.args.sessions

        self.trlog["seen_acc"] = []
        self.trlog["unseen_acc"] = []
```
- Add [[#Things to add]] in the code above.
- Performance of unseen classes increases without `replace_base_fc`, but there's a kind of an error
  related to base classes.

## 2025/05/17
- Detail problem : Base accuracy shows good performance. But when it's tested in incremental session, bad..
- Seems like it's important to think about how i will try to find the problem, not the problem itself.
- Use `replace_base_fc`, and see the similarity between the original `fc` and the mean embedding.
```python helper.py replace_base_fc
        cos_sim_fc_proto = calc_cos_sim(
            model.module.fc.weight.data[: args.base_class], proto_list
        )
        print("###########################")
        print(f"cosine similarity between fc and proto : {cos_sim_fc_proto}")
        print("###########################")
```
- Think just using `replace_base_emb` is the correct method.

## 2025/05/18
- `get_avg_emb` in `helper.py` in `test` takes too much time.
- This is because it's only needed once in the original code, but now it's replacing it for all the batches.
- I should make a temporary fc to store the average embeddings.

## 2025/05/19

### Matrix shape mismatch-2025/05/19
- Error occurs in the second epoch's base session in `get_loss_from_embeddings`.
- Shape of `emb` in `get_loss_form_embeddings` is `(128, 35)`, which is same to the shape of `logits`.
  - Maybe `model.mode` is not `"encoder"`?
  - Let's print the value of `model.mode`.
> [!note]
> By the way, this kind of thinking is very good. When a problem occurs later, try to do what
> I did to solve this problem.
- `model.mode` was set to `avg_cos` in the second base train.
- Set `model.module.mode` to `"encoder"` after `incremental_test`.
```python fscil_trainer.py base_session
                # incremental test
                self.incremental_test(args, session, epoch, optimizer, trainloader)
                self.model.module.mode = "encoder"
```
> [!success] Solved!
