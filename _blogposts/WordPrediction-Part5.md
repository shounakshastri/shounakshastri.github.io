---
title: "Word Prediction Model - PART 5 - Lessons learnt"
collection: blogposts
type: blogposts
excerpt: This is Part 5 , the final part, in the series where we build and deploy a text prediction app using R. In this part we will reflect upon what we have built and discuss the the things we got right and the things which could have been better. 
permalink: /blogposts/Word Prediction Model - PART 5 - Lessons learnt
tags: 
    - Word Prediction
    - Machine Learning
    - NLP
    - R
    - Smoothing
date: 2020-10-25
---

This is the last part in the series where we have now built and deployed a word prediction app. ([You can access it here.](https://shounakshastri.shinyapps.io/word_prediction/)) If you haven't seen the previous parts, you can access them through these links:

[Part 1 - Figuring out the problem at hand](https://shounakshastri.github.io/blogposts/Word%20Prediction%20Model%20-%20PART%201%20-%20Figuring%20out%20the%20problem%20at%20hand)

[Part 2 - Exploring the dataset](https://shounakshastri.github.io/blogposts/Word%20Prediction%20Model%20-%20PART%202%20-%20Exploratory%20Data%20Analysis)

[Part 3 - Creating the model](https://shounakshastri.github.io/blogposts/Word%20Prediction%20Model%20-%20PART%203%20-%20Model%20Building)

[Part 4 - Deploying the model](https://shounakshastri.github.io/blogposts/Word%20Prediction%20Model%20-%20PART%204%20-%20Deploying%20the%20model)

Part 5 - Lessons learnt

## Introduction

> Learning is experience. Everything else is just information. - Albert Einstein

Welcome to the final blog in this series. We have now built and deployed [our word prediction application](https://shounakshastri.shinyapps.io/word_prediction/)). Upon spending some time with the app, we can see that some things could have been done better. In this part, we will discuss the strengths and weaknesses of our app, what went wrong and how we should prepare for the next time we encounter a similar problem.

## Strengths

Let's see what works well first.

### Simplicity

The app is easy to use. This is because we have limited the number of user interactions to 1. So, even if the instructions were not available, the user would have instinctively clicked on the text box.

The user doesn't have to press a button or navigate any menus. He/She simply enters the text and the output is presented. Also, the app shows <span style="color:blue">_---thinking---_</span> when it is waiting for the user to input a complete word. This serves as an indication to the user that either the n-gram phrase is not present in the library or the word is misspelled/incomplete.

### Size

As of writing this post, the size of the app is around 6.3MB. This is good as we still have lots of space if we decide to expand our n-gram library.

### Speed

The speed of prediction is quite good. The app doesn't make the user wait while it's processing the input. This can be attributed partly to the size of our n-gram libraries and the great integration that Shiny provides with R. 

## Weaknesses

Now for the scope of improvement.

### Prediction Accuracy

Well, the prediction accuracy is not great. This is because of the following reasons

**1. We filtered lot of words to reduce the space and processing time.**

The accuracy would have been better if we had kept all the n-grams. But in order to save space and time required to go search through the library, we removed the n-grams with frequencies less than 6. This reduced our ability to predict simply because we didn't have a more exhaustive library.

**2. We haven't implemented smoothing which removes the possibility of adding new n-grams entered by the user.**

Smoothing is implemented when there is a chance that an unseen event would occur. For example, our libraries might contain the following quadgram phrases:

- I want to eat
- I want to play
- I want to go
- I want to have

But, there is a chance that the user might want to do something other than the four options given above. Such as "I want to sing". But since the word "sing" is not present in our library, the app cannot predict it. Smoothing considers this and reserves a small probability for n-grams which are not present in the training data. This would have been great for our application because there is a high possibility that someone would write a phrase which is not stored in our library.

Not implementing Smoothing would probably OK if our dataset were so large that it would cover all possible n-grams in the English language. Then the probability of a phrase unknown to our system would be 0. This is not the case for our app. Our app has access to a corpus which is limited to a certain number of blogs, news articles and tweets and, on top of that, we filtered out phrases which occur less frequently, thus reducing our data further.

This application, absolutely, without a doubt, should have smoothing.

### New phrases

What happens when a new phrase is entered? The application checks the n-grams and outputs the wrong word. However, it would be great if every new phrase is added to the library instead. The frequency and order of the phrases in the library can be made dynamic so that our app doesn't rely only on the filtered n-grams from our corpus. Every time a phrase is entered, the app would increase the frequency of the associated n-grams by 1 and adjust the library accordingly. If the phrase is new, then the app should add it to the library instead of ignoring it. This would increase the size of the library but improve the prediction accuracy.

### Number of predictions

Our app only gives 1 word as the output. It would be better if the app gave, maybe, the top 3 or 4 words instead. Just to give the user a choice.  

## How should we prepare next time we encounter such a problem

First of all, we need to get our hardware in order. The processor and the GPU (although both are very old) were not being utilized to their potential. The bottleneck was at the RAM because the RAM was constantly at 95%+ utilization. A larger sized RAM would not only make the training phase faster, it would also help in increasing the size of our n-gram library resulting in a better prediction.