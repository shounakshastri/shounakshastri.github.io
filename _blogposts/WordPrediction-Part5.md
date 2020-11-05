---
title: "Word Prediction Model - PART 5 - Lessons learnt"
collection: blogposts
type: blogposts
excerpt: [THIS PAGE IS UNDER CONSTRUCTION] This is Part 5 , the final part, in the series where we build and deploy a text prediction app using R. In this part we will reflect upon what we have built and discuss the the things we got right and the things which could have been better. 
permalink: /blogposts/Word Prediction Model - PART 5 - Lessons learnt
tags: 
    - Word Prediction
    - Machine Learning
    - NLP
    - R

date: 2020-10-22
---

<span style="color:orange"> **This blog post is under construction.**</span>

This is the last part in the series where we have now built and deployed a word prediction app. ([You can access it here.](https://shounakshastri.shinyapps.io/word_prediction/)) If you haven't seen the previous parts, you can access them through these links:

[Part 1 - Figuring out the problem at hand](https://shounakshastri.github.io/blogposts/Word%20Prediction%20Model%20-%20PART%201%20-%20Figuring%20out%20the%20problem%20at%20hand)

[Part 2 - Exploring the dataset](https://shounakshastri.github.io/blogposts/Word%20Prediction%20Model%20-%20PART%202%20-%20Exploratory%20Data%20Analysis)

[Part 3 - Creating the model](https://shounakshastri.github.io/blogposts/Word%20Prediction%20Model%20-%20PART%203%20-%20Model%20Building)

[Part 4 - Deploying the model](https://shounakshastri.github.io/blogposts/Word%20Prediction%20Model%20-%20PART%204%20-%20Deploying%20the%20model)

Part 5 - Lessons learnt

## Introduction

> Learning is experience. Everything else is just information. - Albert Einstein

Welcome to the final blog in this series. We have now built and deployed [our word prediction application](https://shounakshastri.shinyapps.io/word_prediction/)) and although it's simple, we could have done some things better. In this part, we will discuss the strengths and weaknesses of our app.

## Strengths

Let's see what works well first

### Simplicity

The app is easy to use. This is because we have limited the number of places where a user can interact to 1. So, even if the instructions were not available, the user would have instinctively clicked on the text box.

The user doesn't have to press a button or navigate any menus. He/She simply enters the text and the output is presented. Also, the app shows <span style="color:blue">_---thinking---_</span> when it is waiting for the user to input a complete word. This serves as an indication to the user that either the n-gram phrase is not present in the library or the word is misspelled/incomplete.

### Size

As of writing this post, the size of the app is around 6.3MB. This is good as we still have lots of space if we decide to expand our n-gram library.

### Speed

The speed of prediction is quite good. The app doesn't make the user wait while it's processing the input.

## Weaknesses

Now for the scope of improvement

### Prediction Accuracy

Well, the prediction accuracy is not great. This is because of the following reasons

**1. We filtered lot of words to reduce the space and processing time.**

The tokenization process takes a longer time to execute as the value of n increases. Also, the file size increases as one sentence is split into multiple phrases usually with repeated words.

For example:



**2. We haven't implemented smoothing which removes the possibility of adding new n-grams entered by the user.**

