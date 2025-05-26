---
id: manifold_mixup_lpc
aliases: []
tags: []
---

# manifold_mixup_lpc

## 2025/04/22
- There could be two ways of applying manifold mixup.
1. Instance level mixup, and make two more embedding vectors with formant and residual.
2. Prototype level mixup.
- Should do number 1 first.
- The code below is the part that gets the speech based classification loss.
```python helper.py base_train
        loss = F.cross_entropy(logits, train_label)
```
- The code below is the part that gets the speech embeddings.
```python helper.py base_train
        logits = F.linear(
            F.normalize(emb, p=2, dim=-1),
            F.normalize(model.module.fc.weight, p=2, dim=-1),
        )
```
- The code below is the part that gets the speech embedding with feature extractor.
```python base_train
        emb, emb_2d = model(data)
```
- The part that gets the formant and residual embedding.
```python helper.py base_train
            emb_res, emb_res_2d = model(res_data)
            emb_a_freq, emb_a_freq_2d = model(a_freq_data)
```
- Have to make two more new vectors with these three different kinds of embeddings.
- Lean `emb` vector to the side of the `emb_a_freq` and `emb_res` each, so that I can get two more embeddings.
  - Should set the hyper-parameter(how much I'll lean the vector).
- Right after `emb` is made, make augmented vectors.
- The code below is the calibration part, and think it'll be good to reference this code to make 
  my LPC augmentation code.
```python Network.py soft_calibration
    def soft_calibration(self, args, session):
        base_protos = self.fc.weight.data[: args.base_class].detach().cpu().data
        base_protos = F.normalize(base_protos, p=2, dim=-1)

        cur_protos = (
            self.fc.weight.data[
                args.base_class + (session - 1) * args.way : args.base_class
                + session * args.way
            ]
            .detach()
            .cpu()
            .data
        )
        cur_protos = F.normalize(cur_protos, p=2, dim=-1)

        weights = torch.mm(cur_protos, base_protos.T) * args.softmax_t
        norm_weights = torch.softmax(weights, dim=1)
        delta_protos = torch.matmul(norm_weights, base_protos)

        delta_protos = F.normalize(delta_protos, p=2, dim=-1)

        updated_protos = (
            1 - args.shift_weight
        ) * cur_protos + args.shift_weight * delta_protos

        self.fc.weight.data[
            args.base_class + (session - 1) * args.way : args.base_class
            + session * args.way
        ] = updated_protos
```
- Added the argument in `train.py`.
```python train.py
    parser.add_argument("-shift_weight_lpc", type=float, default=0.1)
```
- Normalize both vectors(speech, formant or speech, residual)
- Then `(1 - shift)*speeech + shifht*formant`.

1. Normalize the embeddings.
```python helper.py base_train
        emb_norm = emb / torch.unsqueeze(
            torch.torch.norm(emb, p=2, dim=1), dim=1
        ).expand(emb.shape)

        if args.load_from_file == 1:
            emb_a_freq_norm = emb_a_freq / torch.unsqueeze(
                torch.torch.norm(emb_a_freq, p=2, dim=1), dim=1
            ).expand(emb_a_freq.shape)
            emb_res_norm = emb_res / torch.unsqueeze(
                torch.torch.norm(emb_res, p=2, dim=1), dim=1
            ).expand(emb_res.shape)
```
2. LPC based augmentation.
```python helper.py base_train
            # LPC augmentation
            emb_aug_a_freq = (
                1 - args.shift_weight_lpc
            ) * emb_norm + args.shift_weight_lpc * emb_a_freq_norm
            emb_aug_res = (
                1 - args.shift_weight_lpc
            ) * emb_norm + args.shift_weight_lpc * emb_res_norm
```
3. Get logits from LPC augmented embeddings.
```python helper.py base_tarin
        if args.load_from_file == 1:
            logits_aug_a_freq = F.linear(
                F.normalize(emb_a_freq_norm, p=2, dim=-1),
                F.normalize(model.module.fc.weight, p=2, dim=-1),
            )
            logits_aug_res = F.linear(
                F.normalize(emb_res_norm, p=2, dim=-1),
                F.normalize(model.module.fc.weight, p=2, dim=-1),
            )
```
4. Get loss from LPC augmented embeddings.
```python helper.py base_train
            loss_aug_a_freq = F.cross_entropy(logits_aug_a_freq, train_label)
            loss_aug_res = F.cross_entropy(logits_aug_res, train_label)
```
5. Add these in total loss.
```python helper.py base_train
            total_loss = (
                loss
                + args.shift_weight_lpc * (loss_aug_a_freq + loss_aug_res)
```
> [!note] Experiment result
> [[Experiments#2025/04/22]]

## 2025/05/07
> [!important]
> All the things I need to do is in `base_train` in `helper.py`.
- There are three big things I need to do.
  1. Make the guideline of formant and residual embeddings similarity.
    - You can use `make_similarity_matrix` in `analysis/class_similarity.py`.
  2. Make augmented embeddings.
    - We need two types of augmented embeddings: ==replace== and ==add==.
  3. Make dummy_classifier for both *replace-embeddings* and *add-embeddings*.
- Follow the original code that makes augmented embeddings and dummy classifier to make your own code.
- 1, 2 is done. Need to make code that does 3.
> [!note]
> One thing to think about is, maybe I can use the feature generator method used in FSKIL.
> Feature generator is used Instead of using random initialization for the dummy classifier.
>  - Should need to think about whether using feature generator is good or not.

### Make the guideline of formant and residual embeddings similarity
- Look at the code. Too many things added to mention on this note.

### Make augmented embeddings
- Code below is the part that makes the new virtual embeddings.
```python helper.py base_train
        # LPC manifold mixup
        emb_res_replace = (
            emb_norm - emb_res_norm + emb_res_norm[randomly_chosen_idx_tensor]
        )
        emb_res_add = emb_norm + emb_res_norm[randomly_chosen_idx_tensor]
```
### Make dummy_classifier for both replace-embeddings and add-embeddings
- The code below is the part that initializes the virtual prototypes.
```python Network.py __init__
        self.fc_res_replace = nn.Linear(
            self.num_features, self.args.num_classes, bias=False
        )
        self.fc_res_add = nn.Linear(
            self.num_features, self.args.num_classes, bias=False
        )
```

- The code below is the part that gets the logits.
```python helper.py base_train
        logits = F.linear(
            F.normalize(emb, p=2, dim=-1),
            F.normalize(model.module.fc.weight, p=2, dim=-1),
        )
```
- Now the logits are got only with comparing the embeddings and the base prototypes, but to apply my idea,
  I should concatenate the base prototypes and the virtual prototypes to get logits.
- Then the size of the logits will increase.
- The code below is the way pseudo label was made in the original `comp` code.
```python helper.py base_train
        pseudo_label = train_label + args.base_class
```
> [!note]
> I can think of two different ways:
> 1. Do not concatenate the dummy classifier while training the speech prototypes(just like `comp`).
> 2. Opposite to 1.
- The code below is the part that gets logits with augmented data in `comp`. Maybe I can do the same thing.
```python helper.py base_train
        emb_aug1 = F.linear(
            F.normalize(emb_aug, p=2, dim=-1),
            F.normalize(model.module.fc.weight, p=2, dim=-1),
        )
        emb_aug2 = F.linear(
            F.normalize(emb_aug, p=2, dim=-1),
            F.normalize(dummy_classifier, p=2, dim=-1),
            # dummy_classifier : compositional information vectors
        )
        # emb_aug1 : cosine sim between emb_aug and base prototypes
        # emb_aug2 : cosine sim between emb_aug and comp information

        emb_aug_all = torch.cat([emb_aug1[:, : args.base_class], emb_aug2], dim=1)
        logits_aug = args.temperature * emb_aug_all
        # logits_aug : cosine sim of augmented data

        pseudo_label = train_label + args.base_class
```
- Let's only think about `replace` now. Think about `add` later.
- 1, 2 in [[#2025/05/07]] are done for `add`.
- Almost of the things based on `replace` is readly, but this the work is being done from the first eopch.
- So I need to fix it to work with half of the full epochs.
- Now just test the model with `strict` option set to False in `load_state_dict`.
  - This works. Let's just run this code with 30 epochs and go home. Fix it later.
  - I should also need to think about `replace_base_fc`.
> [!note]
> Test experiment result : [[Experiments#2025/05/07]]

## 2025/05/08
- I should make the code work without a pre-trained model.
  - Added `if epoch + 1 == args.epochs_base // 2:` to make this work.
- I also need to think about using feature generator.

## 2025/05/09

### Feature generator-2025/05/09
- Use `dummy_classifier` for virtual prototypes.
- The code below is the part that gets the dummy_classifier for residual replacement.
```python Network.py CompExtractor
class CompExtractorLPC(nn.Module):
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
```python Network.py __init__
        self.set_func_lpc = CompExtractor(self.num_features, self.args.base_class)
```
- The code below is the part that calculates the cosine similarity between virtual speech embeddings
  and the prototypes.
```python helper.py base_train
            logits_res_replace_replaced_classes = F.linear(
                F.normalize(emb_res_replace, p=2, dim=-1),
                F.normalize(model.module.fc_res_replace.weight, p=2, dim=-1),
            )
```
- Should change `model.module.fc_res_replace.weight` to `dummy_classifier_lpc`.
  - Or I can make this optional.
```python helper.py base_train
            if args.use_feature_generator_for_res_replacement == 1:
                logits_res_replace_replaced_classes = F.linear(
                    F.normalize(emb_res_replace, p=2, dim=-1),
                    F.normalize(dummy_classifier_lpc, p=2, dim=-1),
                )
            else:
                logits_res_replace_replaced_classes = F.linear(
                    F.normalize(emb_res_replace, p=2, dim=-1),
                    F.normalize(model.module.fc_res_replace.weight, p=2, dim=-1),
                )
```
- Now let's try 10 epochs with this code.
  > Result : [[Experiments#2025/05/09]]

### Get speech embedding loss with dummy classifier
- The code below is the part that gets the logits from speech embeddings.
```python helper.py base_train
        logits = F.linear(
            F.normalize(emb, p=2, dim=-1),
            F.normalize(model.module.fc.weight, p=2, dim=-1),
        )
```
- The code below is the modified one.
```python helper.py base_train
        logits = F.linear(
            F.normalize(emb, p=2, dim=-1),
            F.normalize(model.module.fc.weight, p=2, dim=-1),
        )
        if args.use_dummy_classifier_for_speech == 1:
            if args.use_feature_generator_for_res_replacement == 1:
                logits_dummy = F.linear(
                    F.normalize(emb, p=2, dim=-1),
                    F.normalize(dummy_classifier_lpc, p=2, dim=-1),
                )
            else:
                logits_dummy = F.linear(
                    F.normalize(emb, p=2, dim=-1),
                    F.normalize(model.module.fc_res_replace.weight, p=2, dim=-1),
                )
            logits = torch.cat([logits[:, : args.base_class], logits_dummy], dim=1)
```
- 10 epochs with this code.
  > Result : [[Experiments#2025/05/09]]

## 2025/05/10

### Structure Vector-2025/05/10
- The code below is the function that gets structure vector and the projection vector of the speech embedding
  to the vector span of formant and residual embedding.
```python manifold_mixup_test.py get_structure_vector
def get_structure_vector(speech_emb, formant_emb, residual_emb):
    G = torch.tensor([[torch.dot(formant_emb, formant_emb), torch.dot(formant_emb, residual_emb)],
                      [torch.dot(formant_emb, residual_emb), torch.dot(residual_emb, residual_emb)]])
    b = torch.tensor([torch.dot(speech_emb, formant_emb), torch.dot(speech_emb, residual_emb)])
    lamdas = torch.linalg.solve(G, b)

    projection_vector = lamdas[0] * formant_emb + lamdas[1] * residual_emb

    structure_vector = speech_emb - projection_vector

    return structure_vector, projection_vector
```
- Ask GPT if you don't remember how this code is made.
- The code below is the part that uses this function.
```python manifold_mixup_test.py main
    target_structure_vector, target_projection_vector = get_structure_vector(target_emb, target_formant_emb, target_residual_emb)
    comparing_structure_vector, comparing_projection_vector = get_structure_vector(target_emb, target_formant_emb, target_residual_emb)

    cos_sim_target_comp = calc_cos_sim(target_emb, comparing_emb)

    rec_comparing_emb = target_formant_emb + adding_residual_emb + comparing_structure_vector
    cos_sim_rec_comp_comp = calc_cos_sim(rec_comparing_emb, comparing_emb)

    print(f"target-comp : {cos_sim_target_comp}")
    print(f"rec_comp-comp : {cos_sim_rec_comp_comp}")
```

### Similarity of structure vector between classes-2025/05/10
- Make this code in `class_similarity.py`.
- Make the `[35, 256]` sized vector set of structure vectors.
- I need to do this to make structure vector considering the batch.

## 2025/05/12

### structure_vector-2025/05/12
- Made the code that gets structure vector work with the batch.
```python helper.py get_structure_vector
def dot(emb1, emb2):
    # shape of emb : (batch, dim) or (dim)
    return torch.sum(emb1 * emb2, dim=-1)


def get_structure_vector(speech_emb, formant_emb, residual_emb):
    if len(speech_emb.shape) > 1:
        batch_size = formant_emb.shape[0]
        dot_result = torch.cat(
            [
                dot(formant_emb, formant_emb).unsqueeze(1),
                dot(formant_emb, residual_emb).unsqueeze(1),
                dot(formant_emb, residual_emb).unsqueeze(1),
                dot(residual_emb, residual_emb).unsqueeze(1),
            ],
            dim=-1,
        )
        G = dot_result.view(batch_size, 2, 2)
        b = torch.cat(
            [
                dot(speech_emb, formant_emb).unsqueeze(1),
                dot(speech_emb, residual_emb).unsqueeze(1),
            ],
            dim=-1,
        )
    else:
        G = torch.tensor(
            [
                [
                    torch.dot(formant_emb, formant_emb),
                    torch.dot(formant_emb, residual_emb),
                ],
                [
                    torch.dot(formant_emb, residual_emb),
                    torch.dot(residual_emb, residual_emb),
                ],
            ]
        )
        b = torch.tensor(
            [torch.dot(speech_emb, formant_emb), torch.dot(speech_emb, residual_emb)],
        )

    lamdas = torch.linalg.solve(G, b)

    if len(speech_emb.shape) > 1:
        projection_vector = (
            lamdas[:, 0].unsqueeze(1).repeat(1, formant_emb.shape[1]) * formant_emb
            + lamdas[:, 1].unsqueeze(1).repeat(1, residual_emb.shape[1]) * residual_emb
        )
    else:
        projection_vector = lamdas[0] * formant_emb + lamdas[1] * residual_emb

    structure_vector = speech_emb - projection_vector

    return structure_vector, projection_vector
```
- The code below is the part that uses `get_structure_vector`.
```python helper.py base_train
        # get structure vector
        structure_vector, projection_vector = get_structure_vector(
            emb_norm, emb_a_freq_norm, emb_res_norm
        )

        # if epoch + 1 > args.epochs_base // 2:
        if epoch + 1 > args.epoch_guideline:
            randomly_chosen_idx_tensor = randomly_choose_different_idx(
                args, train_label, mixup_matching_guideline_list
            )

            # LPC manifold mixup
            emb_res_replace = (
                emb_a_freq_norm
                + emb_res_norm[randomly_chosen_idx_tensor]
                + structure_vector
            )
```

## 2025/05/13

### Single dummy classifier-2025/05/13
- Let's start with `Network.py`.
- Make a single prototype for a dummy classifier.
```python Network.py __init__
        self.fc_res_replace = nn.Linear(self.num_features, 1, bias=False)
```
- And the code below is the part that gets `emb_res_replace`.
```python helper.py base_train
            emb_res_replace = (
                emb_a_freq_norm
                + emb_res_norm[randomly_chosen_idx_tensor]
                + structure_vector
            )
```
- With `train_label`, only leave the embeddings of "three"..?
- Modified the code above to the code below.
```python helper.py base_train
            emb_res_replace = (
                emb_a_freq_norm
                + emb_res_norm[randomly_chosen_idx_tensor]
                + structure_vector
            )
            keyword_three_indices = (train_label == 19).nonzero(as_tuple=True)[0]
            if keyword_three_indices == []:
                no_res_replace_flag = True
            else:
                no_res_replace_flag = False
                emb_res_replace = emb_res_replace[keyword_three_indices]
```
- Below is the part that uses `no_res_replace_flag`.
```python helper.py base_train
        if not no_res_replace_flag:
            if epoch + 1 > args.epoch_guideline:
                logits_res_replace_base_classes = F.linear(
                    F.normalize(emb_res_replace, p=2, dim=-1),
                    F.normalize(model.module.fc.weight, p=2, dim=-1),
```
- The code below is modified temporarily.
```python helper.py base_train
                # pseudo_label_res_replace = (
                #     train_label + args.base_class
                # )
                pseudo_label_res_replace = train_label[keyword_three_indices] + 1
```
- `mixup_matching_guideline_list` should also be got with a different way.
  - Only to match "three" with ==eight=='s residual embedding.
  - The code below is the modified one.
```python helper.py make_mixup_matching_guideline_list
def make_mixup_matching_guideline_list(args, model):
    # shape of model.module.fc.weight.data : [35, 256]
    mixup_matching_guideline_list = []
    cos_sim_matrix = make_similarity_matrix(
        model.module.fc_residual.weight.data[: args.base_class]
    )
    for i in range(args.base_class):
        mixup_matching_guideline_list.append(torch.argmin(cos_sim_matrix[i, :]).item())

    mixup_matching_guideline_list[18] = 14  # REMOVE THIS LINE

    return mixup_matching_guideline_list
```

### Using lamda-2025/05/13
- The code below is the part that uses lamdas when making a new virtual embeddings.
```python helper.py base_train
            emb_res_replace = (
                lamdas[:, 0].unsqueeze(1).repeat(1, emb_a_freq_norm.shape[1])
                * emb_a_freq_norm
                + lamdas[:, 1].unsqueeze(1).repeat(1, emb_res_norm.shape[1])
                * emb_res_norm[randomly_chosen_idx_tensor]
                + structure_vector
            )
```

### Using new structure vector-2025/05/13
- The code below is the part that gets the new structure vector.
```python helper.py base_train
        # get structure vector
        structure_vector, projection_vector, lamdas = get_structure_vector(
            emb_norm, emb_a_freq_norm, emb_res_norm
        )
        structure_vector_size = torch.sum(
            torch.sqrt(structure_vector * structure_vector), dim=-1
        )
        new_projection_vector = (
            lamdas[:, 0].unsqueeze(1).repeat(1, emb_a_freq_norm.shape[1])
            * emb_a_freq_norm
            + lamdas[:, 1].unsqueeze(1).repeat(1, emb_res_norm.shape[1])
            * emb_res_norm[randomly_chosen_idx_tensor]
        )
        new_structure_vector, _, _ = (
            F.normalize(
                get_structure_vector(
                    emb_norm, emb_a_freq_norm, emb_res_norm[randomly_chosen_idx_tensor]
                ),
                dim=-1,
            )
            * structure_vector_size
        )
```
- And the part that uses this.
```python helper.py base_train
            emb_res_replace = new_projection_vector + new_structure_vector
```

## 2025/05/14

### Least and most similar embeddings-2025/05/14
- Don't just use least similar embeddings for augmentation.
- Modify the function that gets least similar embedding indices to get least or most similar embeddings' indices.
```python helper.py get_least_or_most_similar_emb_idx
def get_least_or_most_similar_emb_idx(emb1, emb2, least_or_most):
    # shape of emb : (batch_size, 256)
    emb1 = F.normalize(emb1, p=2, dim=-1)
    emb2 = F.normalize(emb2, p=2, dim=-1)
    similarity_matrix = torch.matmul(emb1, emb2.transpose(1, 0))
    if least_or_most == "least":
        least_similar_emb_idx = torch.argmin(similarity_matrix, dim=-1)
    elif least_or_most == "most":
        least_similar_emb_idx = torch.argmax(similarity_matrix, dim=-1)
    else:
        least_similar_emb_idx = torch.argmin(similarity_matrix, dim=-1)
    # shape of least_similar_emb_idx : (batch_size)

    return least_similar_emb_idx
```
- The code below is the part that gets the augmented embeddings.
```python helper.py base_train
            # just replace the residual embedding
            emb_res_replace_least_sim = (
                emb_norm - emb_res_norm + emb_res_norm[least_similar_res_emb_idx_tensor]
            )
            emb_res_replace_most_sim = (
                emb_norm - emb_res_norm + emb_res_norm[most_similar_res_emb_idx_tensor]
            )
            emb_res_replace_a_freq_most_sim = (
                emb_norm
                - emb_res_norm
                + emb_res_norm[most_similar_a_freq_emb_idx_tensor]
            )
```
- Added to more dummy classifiers set in `Network.py`.
```python Network.py __init__
        self.fc_res_replace = nn.Linear(
            self.num_features, self.args.num_classes, bias=False
        )
        # self.fc_res_replace = nn.Linear(self.num_features, 1, bias=False)
        self.fc_res_replace_most_sim = nn.Linear(
            self.num_features, self.args.num_classes, bias=False
        )
        self.fc_res_replace_a_freq_most_sim = nn.Linear(
            self.num_features, self.args.num_classes, bias=False
        )
```
- The code below is the part that gets logits from multiple classifiers.
```python helper.py get_logits_from_mulitple_classifiers
def get_logits_from_mulitple_classifiers(
    args, train_label, epoch, emb, classifier_list, num_stacked_classifiers
):
    logits_list = []
    if epoch + 1 > args.epoch_guideline:
        for fc_weight in classifier_list:
            logits = F.linear(
                F.normalize(emb, p=2, dim=-1),
                F.normalize(fc_weight, p=2, dim=-1),
            )
            logits_list.append(logits[:, : args.base_class])
        logits_replace_all = torch.cat(logits_list, dim=1)
        logits_replace = args.temperature * logits_replace_all

        pseudo_label_replace = train_label + args.base_class * num_stacked_classifiers

        loss_replace = F.cross_entropy(logits_replace, pseudo_label_replace)

        return loss_replace
```
- Get loss from three classifiers(base, least sim res, most sim res, most sim form).
```python helper.py base_train
            if epoch + 1 > args.epoch_guideline:
                classifier_list = [
                    model.module.fc.weight,
                    model.module.fc_res_replace.weight,
                    model.module.fc_res_replace_most_sim.weight,
                    model.module.fc_res_replace_a_freq_most_sim.weight,
                ]
                loss_res_replace_least_sim = get_loss_from_mulitple_classifiers(
                    args,
                    train_label,
                    epoch,
                    emb_res_replace_least_sim,
                    classifier_list,
                    1,
                )
                loss_res_replace_most_sim = get_loss_from_mulitple_classifiers(
                    args,
                    train_label,
                    epoch,
                    emb_res_replace_most_sim,
                    classifier_list,
                    2,
                )
                loss_res_replace_a_freq_most_sim = get_loss_from_mulitple_classifiers(
                    args,
                    train_label,
                    epoch,
                    emb_res_replace_a_freq_most_sim,
                    classifier_list,
                    3,
                )
```
- Below is the part that these three losses getting added to the total loss.
```python helper.py base_train
        if args.vector_based_only == 1:
            total_loss = (
                loss
                + args.loss_aug_res * loss_aug_res
                + args.loss_aug_a_freq * loss_aug_a_freq
                + args.loss_a_freq_cls * loss_a_freq  # formant cls loss
                + args.loss_res_cls * loss_res  # residual cls loss
                + args.loss_lpc * loss_lpc
                + args.loss_lpc_shuffled * loss_lpc_shuffled
                # + args.loss_res_replace_fact * loss_fact
                + args.loss_sim_speech_form * loss_sim_speech_form
                + args.loss_sim_speech_res * loss_sim_speech_res
            )
            if epoch + 1 > args.epoch_guideline:
                total_loss = (
                    total_loss
                    + args.loss_res_replace * loss_res_replace_least_sim
                    + args.loss_res_replace * loss_res_replace_most_sim
                    + args.loss_res_replace * loss_res_replace_a_freq_most_sim
                )
```

## 2025/05/17

### Use residual energy-2025/05/17
- Should extract and save residual data in time domain first.

## 2025/05/20

### Use residual energy-2025/05/20
- I did a lot of things today..
- I trained the model with the ratio of residual energy and the speech embedding, and etc.

## 2025/05/21

### Others-2025/05/21
- [ ] Meeting materials.
  - Use cepstrum to get formant and residual.

### Code-2025/05/21
- [ ] Use cepstrum analysis to get formant and excitation spectrum.
  > Detail : [[cepstrum_dataloader#Use T.Spectrogram-2025/05/21]]
  - Need to check the algorithm with a test code.

## 2025/05/23

### Spectrum boundary detection-2025/05/23
- 15:0:27 : The code below is the part that gets the energy from the spectrum(not the exact energy).
```python spectrum_analysis.py main
    ener = torch.sum(torch.sqrt(torch.exp(spec) * torch.exp(spec)), dim=1)
```
- The code below is the part that gets the difference of the log spectrum(and peak detection).
```python spectrum_analysis.py main
    spec_diff = torch.diff(spec_formant)
    dist_ener = torch.sum(spec_diff * spec_diff, dim=1).to("cpu").numpy()[0]
    spec_dist = torch.sum(spec_diff * spec_diff, dim=1)

    peak_indices, properities = find_peaks(dist_ener, prominence=80)
```
- I also need to detect the start and the end of the speech.
  - 15:11:14 : Think I can just use the first peak along the found peaks with the code above.
    - 15:12:39 : But what if the start of the speech was pretty soon and the code above doesn't find the
      peak at the start of the speech?
  - Think I need to use energy here.

> [!note] 15:29:20 : Things to add in the code
> 1. Find peaks and return indices.
>   - This takes too much time. Maybe I should save the data.
> 2. 16:41:42 : Make it work with 100 coefficients.
- The code above is to make the code work with 100 coefficients.
```python bcresnet.py BCResNets
        self.avg_pool_after_cnn_head = nn.AdaptiveAvgPool2d(
            (20, self.args.num_features_pri)
        )
```
- This works.

## 2025/05/24
- Keep up in [[#Spectrum boundary detection code-2025/05/23]].
- I was editing the code below.
```python helper.py base_train
        for i in range(data.shape[0]):
            spec_diff = torch.diff(a_freq_data[i, :, :])
            dist_ener = (
                torch.sum(spec_diff * spec_diff, dim=1).detach().to("cpu").numpy()[0]
            )
            peak_indices, properities = find_peaks(dist_ener, prominence=500)
            peak_indices = peak_indices[
                ener.detach().to("cpu").numpy()[i, peak_indices + 1] > 20
            ]
            peak_indices = torch.tensor(peak_indices).to("cuda")
            if peak_indices.shape[0] < 3:
                peak_indices = torch.tensor([0, 1, 2])
                print(peak_indices)
            peak_indices = peak_indices.unfold(0, 2, 1)
            print(peak_indices)

            avg_embeddings_list = []
            for j in range(peak_indices.shape[0]):
                avg_embeddings_list.append(
                    torch.mean(
                        emb_2d[i, :, 0, peak_indices[j, 0] : peak_indices[j, 1]], dim=-1
                    )
                )
```

## 2025/05/25
- 18:31:54 : Decided not to use energy.
- Function `extract_peak_pooled_emb` made.
```python utils.py extract_peak_pooled_emb
def extract_peak_pooled_emb(emb, emb_2d, data, a_freq_data):
    emb_peak_pooled = torch.empty(emb.shape).to("cuda")
    peak_indices_batch = []
    emb_2d_peak_pooled = []
    # find peaks from the spectral difference
    for i in range(data.shape[0]):
        spec_diff = torch.diff(a_freq_data[i, :, :])
        dist_ener = (
            torch.sum(spec_diff * spec_diff, dim=1).detach().to("cpu").numpy()[0]
        )
        peak_indices, properities = find_peaks(dist_ener, prominence=500)
        # peak_indices = peak_indices[
        #     ener.detach().to("cpu").numpy()[i, peak_indices + 1] > 0
        # ]
        peak_indices = torch.tensor(peak_indices).to("cuda")
        if peak_indices.shape[0] < 3:
            peak_indices = torch.tensor([0, 0, 0])
        peak_indices = peak_indices.unfold(0, 2, 1)
        peak_indices_batch.append(peak_indices)

        avg_embeddings_list = []
        for j in range(peak_indices.shape[0]):
            emb_2d_peak_pooled_first = torch.mean(
                emb_2d[i, :, 0, peak_indices[j, 0] : peak_indices[j, 1]], dim=-1
            )
            avg_embeddings_list.append(emb_2d_peak_pooled_first.unsqueeze(1))
        avg_embeddings_tensor = torch.cat(avg_embeddings_list, dim=-1)
        emb_2d_peak_pooled.append(avg_embeddings_tensor)
        emb_peak_pooled[i, :] = torch.mean(avg_embeddings_tensor, dim=-1)

        return emb_peak_pooled
```
- This is in `base_train`, `test`, `replace_base_fc` in `helper.py` and `update_fc` in `Network.py`.
