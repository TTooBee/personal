---
id: Scratch
aliases: []
tags: []
---

# Scratch

## 2025-03-08
- Loss 2d modified. Maybe the base training accuracy does not perform that well is because of the different learning rate between vector based processes and the matrix based processes. [[experiments]]
- All I need to think about where I should apply the 2d base learning process is the **test** procedure and the **loss** procedure
- Learning rate 0.005, loss_2d 0.2, ==test with vector embeddings==, ==loss with both vector and matrix embeddings==.
- Should compare this result with the base code.
- Cosine similarity loss may be better at some parts, and CKA may be better at something else. These two do not seem like one is just better than the other one.
- So don't just try to add the loss and make the model do everything, and see the result and analyse it. Maybe CKA can me used to calibration or froward compatible methods or any other things that are not just letting the model do everything.
- Then I need to make the code to analyse the result...
- If I try to analyse two models, vector based(base code) and the matrix based(primitive code) models, how should I analyse it..?
- Think I don't have anything about dummy classifier.

## 2025-03-09
- What should I do now... Now I have the result of the 2d based model and the vector based model, show I think I can compare these two, and see the actual distribution of  the model.
- Think I should separate the mode between 2d-based and the original.
- Now we need to visualize the distribution_dict. But isn't this actually is the exact same thing as the confusion matrix? Yes it is. Don't need to do this. Just see the confusion matrix.
- Seq length can be modified by in `bcresnet.py` file.
- When gram matrix is got from a matrix, **sequence of the matrix does not matter anymore**. Sequence of the vectors in the matrix can not differentiate the matrix. But it the keyword spotting task, sequence do matter because this data is actually temporal data.
- So the thing is, CKA loss may work on image data(since there is no sequence in the images), but won't work in the keyword spotting task.
- Vectors in the matrix should be matched 1 on 1 to consider the sequence of the data.
- One more thing to think when 1 on 1 matching is being done(related to CTC?). People my the same words in various different ways, so should I think about the CTC related thing to match this? Or would the model do everything?
- Then isn't this actually the same from the speech recognition task?
- Element wise multiplication between the prototype and the feature sucks when model being tested.
- Length of the keywords are all different. But in the original code, it just uses the fixed size embeddings. Is there any idea for this to compare the matrices that their lengths are all different?
- What is the actual thing that you want to do with LLM in your local pc? Access to the file system, look what's inside and tell us what to do when we ask about our computer.
- Input and output first. What about getting related to note taking?
- LLM doesn't know what we think of, so maybe obsidian can provide LLM the information about us. Or LLM can do the note taking instead of us.
- I use a lot of scratch papers, so would there be anything to do with this?
- Ask myself, with LLM obsidian. Do I even need to fine-tune the model? Think I can just use it.
- I need to specify the input and the output of the LLM. Since I'm using the QA model, input is the prompt and the output is the answer of the LLM.
- It's kinda hard to think about what to do when first set at the desk, starting to work. Maybe LLM can look at the previous tasks, and predict next things to do.
- Aha moments of human. But the thing is, how can I model the Aha moments?
- First we need some data. Previously written things by human. And just like how SSL models are trained, the LLM is fine-tuned with the previously written notes.
- Or maybe LLM can copy my brain storming procedures.

## 2025-03-12
- Would it be bad to make the vector in a fixed length?
- If I try to apply CTC loss in my code, logits2 doesn't need to be extracted.
  - Maybe leaving logits2 is good. Just change loss calculating part to a conditional block.
- Two Transformer encoder layers.

## 2025-03-14
- Why is the performance of the model learned without transformer layer so bad? It wasn't like this a few days ago..
  - Solved~
- To make Transformer module, should delete all the parts related to logits2. Before logits2 is made, logits should be made in the doubled size.

## 2025-03-18
- Think about test first. If there is no noise, then no problem. But if noisy? Then should think about how noisy lpc can be applied.

## 2025-03-21
- To get 101 sequence length when doing lpc, what can we do...
- Hop length 160, zero pad 160 samples.

## 2025-03-29
- What Should I do now... Masking only the residual spectrum and the formant and match them, and make them as the new class.
- Apply noise only to the speech, and match them with formant and residual embeddings. 

## 2025-03-31
- `replace_base_fc` runs **AFTER** all the base training session is finished. And it replaces the fc with the mean of **all the embeddings** of the training data.

## 2025-04-02
- What am I doing now...
- Consider that I have a speech embeddings, formant embeddings and the residual embeddings.
- What does it mean when the model is using the compositional information in the calibration process?
- I don't want to use just a random learnable parameters.
- There are a lot of combinations to make `emb_rec`.

## 2025-04-03
- Why not make the file tree exactly the same to the project file tree?

## 2025-04-04
- Looking at the visualized result of the embeddings may help design ideas.
- What do I need to do...
- Maybe I can make code that makes the model learn only by the formant embeddings or residual embeddings.

## 2025/04/07
- Think it'll be good to make something that tells **What Server is Doing**. But have no idea how to add this part.
- If possible, then I can ask LLM what I did before with my GPU server.
- Increase the value of modifying window size from 2 to 10.
- Incremental session performance goes terrible when the model is learned with only residual.
- It never can be the result of the residual itself(it's the problem of the code).

## 2025/04/08
- I should make the code work with residual only.
- There's an NaN value in the logits. How is this possible? Maybe I should check the value of the data first.
- `forward_metric` may be the problem since it's the part closely related to `test`.
- Think I solved it. There was no code in `test` that changes nan to a number.

## 2025/04/09
- Everything is being executed in the vector space.

## 2025/04/10
- What do I need to do...
- Server is working but almost done.
- I should study more. Let's read a paper.
- Or I should take some notes about the papers that I read before. Think the code with [[formant_residual#2025/04/10]] works pretty well.
- It does not outperforms comp model though. I should calibrate the vector or do anything after the base session.
- Then I should look at the result of the model(maybe confusion matrix is a good thing to look at).

## 2025/04/11
- What do I do now. Think about things to do. I was looking at the confusion matrix.
- Let's look at the spectrogram of "four" and "forward".
- And the embedding vectors also.
- "four" and "forward" has almost the same structure of voiced sound, but maybe would be a bit different with the temporal pattern.
- And certainly the last "d" sound is the most different part between those two words.
- That means the embedding made with the original model(without Transformer module) should be very similar,
  and the embedding from Transformer module should be a bit different.
- And finally the residual embedding should be different.
- This can be an idea. If the model cannot confidently classify the keyword, then the model can look up for the
  residual embedding.
- Maybe I need a higher LPC order to get the residual that has less information about voiced sound.

## 2025/04/14
- Think I need to look at the actual embedding space.

## 2025/04/16
- The server will end the training till about 4 hours later.
- It'll be good to find another thing to do during this.
- Think of how I should forecast the new classes during the base session.
- "Four" and "forward" is different, and "three" and "tree" is also different, but the model cannot discriminate
  these two when one is in the base session and the other is in the new session.
- So what I need to do is to make a method to forecast the difference between the base session and the new session.
- Rhyme. Same voiced sound, different unvoiced sound.
- I should think about some common patterns during the base session with the pattern I found, I can apply the pattern
  to make a new virtual keyword embedding.
- I have six vectors
  - formant bcresnet
  - formant transformer
  - residual bcresnet
  - residual transformer
- If I use FSKIL to the baseline, and make prototypes(classifier) with formant prototypes and residual prototypes,

## 2025/04/17
> [!note] All keywords
> 1. five : rec form res[!check]
> 2. zero : form rec res[!check]
> 3. yes : rec form res[!check]
> 4. seven : rec res form
> 5. no : rec form res[!check]
> 6. nine : form res rec[!check]
> 7. down : res rec form
> 8. one : form rec res[!check]
> 9. go : rec form res[!check]
> 10. two : res rec form
> 11. stop : res rec form
> 12. six : res rec form
> 13. on : form rec res[!check]
> 14. left : res rec form
> 15. eight : res rec form
> 16. right : rec form res[!check]
> 17. off : res rec form
> 18. four : rec form res[!check]
> 19. three : rec form res[!check]
> 20. up : res rec form
> 21. dog : rec form res[!check]
> 22. wow : form rec res[!check]
> 23. house : rec form res[!check]
> 24. marvin : rec form res[!check]
> 25. bird : rec form res[!check]
> 26. happy : rec form res[!check]
> 27. cat : res rec form
> 28. sheila : rec form res[!check]
> 29. bed : rec res form
> 30. tree : rec form res[!check]
> 31. backward : rec res form
> 32. visual : rec res form
> 33. follow : rec form res[!check]
> 34. learn : form rec res[!check]
> 35. forward : form rec res[!check]
---
- The model doesn't know what will come during the incremental session.
- With a certain pattern, there can be an dynamic calibration.
- But this is not actually about the difference between the model and the 

## 2025/04/18
- Should look at the embedding separately.
- Formant bcresnet
- Formant transformer
- Residual bcresnet
- Residual transformer
- Just compare these four prototype vectors of the keyword ==three== and ==tree== first.
- You need to prove your hypothesis first.
- It'll be a great progress if the hypothesis is proved.
- Transformer embedding vectors are too small. Maybe I can just get rid of it.

## 2025/04/19
> [!important]
> When writing the code, and make a certain feature optional, always try to make the code work properly
> with all the possible options.
---
> [!note] All keywords
> 1. five form
> 2. zero
> 3. yes res
> 4. seven
> 5. no res
> 6. nine
> 7. down
> 8. one form
> 9. go
> 10. two res
> 11. stop res
> 12. six res
> 13. on form
> 14. left res
> 15. eight res
> 16. right form
> 17. off form
> 18. four form
> 19. three form
> 20. up res
> 21. dog form
> 22. wow form
> 23. house form
> 24. marvin form
> 25. bird res
> 26. happy res
> 27. cat res
> 28. sheila
> 29. bed res
> 30. tree
> 31. backward
> 32. visual res
> 33. follow form
> 34. learn
> 35. forward form
---

## 2025/04/22
- Augmentation with formant and residual embedding vectors.
- Then in the same class, there can be two more embeddings.
- And for the intra class mixup, we can just make a vector and use it as a classifier.
- Or can make a vectors with prototype level mixup.

## 2025/04/23
- Why do you think the result gets bad with residual augmentation?
- If this gets bad, then I think it means that it could be good to make a new embedding with 

## 2025/04/30
- Now I need to think of what to do.
- I need to find the evidence that residual and formant have advantage in specific keywords.
