---
title: "Word Prediction Model - PART 1 - Figuring out the problem at hand"
collection: blogposts
type: blogposts
excerpt: This is Part 1 in a series of posts where we build a word prediction application in R. In this post, we lay out our plan of action and decide how to proceed with the project. 
permalink: /blogposts/Word Prediction Model - PART 1 - Figuring out the problem at hand
tags: 
    - Machine Learning
    - NLP
    - R

date: 2020-10-13
---

This post is the first a series where we build a basic word prediction model using R. Here we will do some planning about how we want our project to proceed, setting up our goals and expectations. The series would be divided into 5 parts as given below:

Part 1 - Figuring out the problem at hand

[Part 2 - Exploring the dataset](https://shounakshastri.github.io/blogposts/Word%20Prediction%20Model%20-%20PART%202%20-%20Exploratory%20Data%20Analysis)

[Part 3 - Creating the model](https://shounakshastri.github.io/blogposts/Word%20Prediction%20Model%20-%20PART%203%20-%20Model%20Building)

[Part 4 - Deploying the model](https://shounakshastri.github.io/blogposts/Word%20Prediction%20Model%20-%20PART%204%20-%20Deploying%20the%20model)

Part 5 - Lessons learnt

---

So, welcome to Part 1 of the NLP series where we build a word prediction model using R.

In this post, we will be planning some of the aspects of our project. As a result, <span style="color:green">*this post might be quite wordy and would not involve programming.*</span> But, this kind of planning is probably one of the most important steps as this is where we figure out the goals and  expectations from the project. So, in this part of the series, we would discuss about the problem at hand and fix some parameters for the project like the kind of model that we would use, the platform where we would deploy, possible problems we could encounter, etc. In practice, this kind of planning would be done in a meeting in the presence of your team members. 

## 1. Figuring out the problem 

Ok. So we need to build a program to predict the next word based on the words that a user has typed. This would basically be like most of the modern word predictors in our phones. Here are the goals for our project

- The target user would be a person who frequently types messages, emails, tweets, etc and uses the modern/current form of the English language. This basically means we don't want the predictor to predict words like "thou" or convert normal English to leetspeak like 3ngl!sh. 

- The app should be fast and should require less memory.

- The user interface should be basic and uncomplicated so that it can be used by anybody who knows how to type on a keyboard. 


## 2. Data Collection

The data would heavily influence this problem as the model which we train will output words based on the vernacular used in the dataset. For example, if we were to present this application to a person who writes Shakespearean english, we would need to use a corpus which would have text written in Shakespearean english. In this case, we are targetting users who use normal day-to-day english language. So, a corpus containing text from blog posts, news articles, tweets, etc should work well. Such corpus would be easy to aquire for free as most of the popular text mining libraries like [NLTK](https://www.nltk.org/) and [tm](https://cran.r-project.org/web/packages/tm/tm.pdf) include some corpora and training models. I have used the Swiftkey dataset which contains text files of blogs, news and tweets in this project. But the user can use any corpus which is written in modern english. 

## 3. The model to be used

The most suitable model in this case would be using the [n-gram](https://en.wikipedia.org/wiki/N-gram) model with [Katz's Backoff](https://en.wikipedia.org/wiki/Katz%27s_back-off_model) to predict the next word.

### **3.1 What are n-grams?**

n-grams are phrases which are formed by splitting a sentence into groups of 'n' words. For example, consider the sentence below

```
I am really excited for the new stormlight archive book.
```

The 3-grams or trigrams for this sentence would be:

```
"I am really"
"am really excited"
"really excited for"
.
.
.
"stormlight archive book"
```

### **3.2 Back-off Algorithm**

Back-off algorithm is used to search the input phrase in the the list of (n-1)-grams if we don't find the exact match in our list of n-grams. The n-gram phrases are arranged according to their frequency of occurrences and the output would be the most common phrase in the list containing the words in that exact order. This means if the user has entered the phrase
```
My name
```
and there is no phrase which resembles "my name ____" in our 3-gram list, then check for the phrase "name ___" in our 2-grams list. 

This means we would have to maintain multiple lists of n-gram phrases. 

<span style="color:red"> *This would probably require lots of storage and might cause problems in deployment.*</span>

To avoid this, we would only keep the n-grams which occur more than a certain number of times. We can decide while training the model based on the kinds of phrases the model covers.

## 4. Model deployment 

R has a pretty solid deployment offering in the form of [Shiny](https://www.analyticsvidhya.com/blog/2016/10/creating-interactive-data-visualization-using-shiny-app-in-r-with-examples/). It can be used to deploy a simple app on web without the knowledge of HTML, CSS or JavaScript for free. This means we are fine as long as we know R. The app can be reactive i.e. the next word can be displayed without the need of pressing a button or creating a distinct event. This would make the app easy to use. I have created a very basic design in MS Paint that would probably piss people off but it is what it is and whatever it is is given below 🤦‍♂️. 

![Basic App Layout](/images/ShinyAppLayout.jpg)

---
Now that we have laid out the plan, we can move on to writing the code and creating the model.

In [PART 2](https://shounakshastri.github.io/NLP/Word%20Prediction%20Model%20-%20PART%202%20-%20Exploratory%20Data%20Analysis) we will do a small exploratory analysis of our data and try to understand where we might run into problems like hardware bottlenecks, storage issues, etc.
