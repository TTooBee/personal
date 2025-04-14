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
> 18. four [!check]  confusing to forward
> 19. three [!check]  confusing to tree
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
> 30. tree [!check]  confusing to three
> 31. backward
> 32. visual
> 33. follow
> 34. learn
> 35. forward  [!check]  confusing to four
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
> [!tip] Further idea
> [[Scratch#2025/04/11]]
---
- Then I should extract LPC related features with a higher order(maybe 16 or 20)
  > [!note]
  > Code : [[extract_lpc_data#2025/04/11]]
