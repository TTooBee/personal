---
id: confusing_keywords
aliases: []
tags: []
---

# confusing_keywords

## Keywords
---
> [!note] All keywords
> 1. five
> 2. zero
> 3. yes
> 4. seven
> 5. no
> 6. nine
> 7. down
> 8. one
> 9. go
> 10. two
> 11. stop
> 12. six
> 13. on
> 14. left
> 15. eight
> 16. right
> 17. off
> 18. four [!check]  confused with forward
> 19. three [!check]  confused with tree
> 20. up
> 21. dog
> 22. wow
> 23. house
> 24. marvin
> 25. bird
> 26. happy
> 27. cat
> 28. sheila
> 29. bed
> 30. tree [!check]  confused with three
> 31. backward
> 32. visual
> 33. follow
> 34. learn
> 35. forward  [!check]  confused with four
---

## Confusing Pronunciations

| Similar Vowel Sounds                   |
| -------------------------------------- |
| - five, nine                           |
| - right, five                          |
| - bird, learn                          |

| Similar Initial Consonants             |
| -------------------------------------- |
| - two, tree                            |
| - forward, four, follow                |
| - five, forward                        |
| - bed, bird                            |

| Similar Ending Sounds                  |
| -------------------------------------- |
| - dog, bird, bed, backward, forward    |
| - on, down, learn, marvin              |
| - up, stop                             |

| Short One-Syllable Words               |
| -------------------------------------- |
| - go, no                               |
| - on, off                              |
| - yes, no                              |
| - up, dog                              |
| - five, nine                           |

| Similar Rhythm and Length              |
| -------------------------------------- |
| - seven, eleven                        |
| - happy, marvin                        |
| - forward, backward                    |
| - visual, follow                       |

| Easily Confused Sounds                 |
| -------------------------------------- |
| - nine, five                           |
| - six, fix                             |
| - left, right                          |
| - forward, follow                      |
| - backward, forward                    |

## 2025/04/11
- Think we first need to look at the embeddings of speech, formant, and residual.
---
> [!note]
> Also, take a look at the result of the model only made with the formant embeddings or the residual embeddings only.
> Result for the form or res only model : [[Experiments#2025/04/09]]
--- 
[!tip] Further idea
> [[Scratch#2025/04/11]]
---
- Then I should extract LPC related features with a higher order(maybe 16 or 20)
  > [!note]
  > Code : [[extract_lpc_data#2025/04/11]]

## 2025/04/16
- Have to look at the speech, formant, residual prototypes of "four"/"forward", and "three"/"tree".
> [!note]
> Look at [[Scratch#2025/04/17]]

## 2025/04/18
- Think about *tree* and *three*.
---
> [!hint] Hypothesis 1
> There could be a meaning in each type of embeddings.
> - ==Formant embeddings== after ==bcresnet==(f-b) : Voiced sound itself.
> - ==Formant embeddings== after ==Transformer==(f-t) : Position or the combination of the voiced sound.
> - ==Residual embeddings== after ==bcresnet==(r-b) : Unvoiced sound itself.
> - ==Residual embeddings== after ==Transformer==(r-t) : Position or the combination of the voiced sound.
---
> [!hint] Hypothesis 1-1
> - According to Hypothesis 1, tree and three should have similar ==f-b, f-t, r-t(not sure with this)==,
>   and different ==r-b==.
---
- The values below are the sizes of the embeddings.
--- 
> [!note] Size of the embeddings
> size of the vector sb : 3.9333059787750244
> size of the vector st : 0.061943747103214264
> size of the vector fb : 3.1747689247131348
> size of the vector ft : 0.07387151569128036
> size of the vector rb : 0.42519915103912354
> size of the vector rt : 0.05004102364182472
---
- As shown above, the size of the prototypes from the Transformer layer is very small.
  - Maybe I don't need to use the Transformer layer.
- The values below are the sizes of the embeddings.
---
> [!note]
> cos_sim_sb : 0.9912543296813965
> ~~cos_sim_st : 0.8509209752082825~~ *this seems meaningless because the size of the vector is too small*
> cos_sim_fb : 0.990449070930481
> ~~cos_sim_ft : 0.6878248453140259~~ *this seems meaningless because the size of the vector is too small*
> cos_sim_rb : 0.9217929840087891
> ~~cos_sim_rt : 0.9978857636451721~~ *this seems meaningless because the size of the vector is too small*
---
- I should make the power of the residual embedding stronger, and I think this can be done by
  ==prototype level manifold mixup== used in [[Forward_Compatible_Few-Shot_Class-Incremental_Learning]].
- Think I need to study more about the true meaning of the dual labeling used in FACT, prototype level mixup.
- Think about the example below.
---
> [!example]
> - Think about the speech prototype of the keyword "tree".
> - Use the formant prototype, add another class' residual prototype to the formant prototype.
>   - Should also think about how to choose the class for the residual prototype.
---

## 2025/04/22
- Select a class that the formant embedding is similar and the residual embedding is different.
- Augment the speech embeddings by adjusting the speech embedding to the formant and to the residual.
- Then I can have 2 more augmented embeddings.
- Make a virtual instance by applying manifold mixup, and compare this with virtual prototype.
---
> [!example]
> Instance level mixup between 2 different classes, inside a batch.
> Need to think about 2 different means.
> 1. The virtual prototypes are made by applying the manifold mixup on the base prototypes.
> 2. The virtual prototypes are made just as a new `fc`.
---
- Think the second way is a proper way of doing this, but still need to try both.
  > Code : [[manifold_mixup_lpc#2025/04/22]]

## 2025/04/23
- Speech embedding - residual embedding + different class' residual embedding.
- The result gets bad with LPC feature augmentation.
  > The last experiment in [[Experiments#2025/04/23]].
