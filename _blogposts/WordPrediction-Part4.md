---
title: "Word Prediction Model - PART 4 - Deploying the model"
collection: blogposts
type: blogposts
permalink: /blogposts/Word Prediction Model - PART 4 - Deploying the model
tags: 
    - WWord Prediction
    - Machine Learning
    - NLP
    - R
    - Model Deployment
    - Shiny

date: 2020-10-13
---

This post is the fourth in the series where we build a basic word prediction model using R. At this stage we have already built the model. In this post we will be deploying it on Shiny. [You can check the app using this link.](https://shounakshastri.shinyapps.io/word_prediction/). If you have missed any of the previous posts, you can click on the links below to check those out. 

[Part 1 - Figuring out the problem at hand](https://shounakshastri.github.io/blogposts/Word%20Prediction%20Model%20-%20PART%201%20-%20Figuring%20out%20the%20problem%20at%20hand)

[Part 2 - Exploring the dataset](https://shounakshastri.github.io/blogposts/Word%20Prediction%20Model%20-%20PART%202%20-%20Exploratory%20Data%20Analysis)

[Part 3 - Creating the model](https://shounakshastri.github.io/blogposts/Word%20Prediction%20Model%20-%20PART%203%20-%20Model%20Building)

Part 4 - Deploying the model

Part 5 - Conclusion and future scope for improvement

---

So, welcome to Part 4 of our series. At this stage, we have 4 files which contain bigrams, trigrams, quadgrams and pentagrams along with the prediction file which contains the model to predict subsequent words. We will need these five files in a separate folder. 

First, we need `shiny package` to deploy the app.

To make it easier in the app, we will transfer all the imports from the `prediction_code.R` file to the `app.r`. We will also include the prediction file as the source in the app.

```r
library(shiny)
suppressPackageStartupMessages({
    library(tidyverse)
    library(stringr)
})

source("prediction_code.R")
```

The app has two sections:

1. front end (ui)
2. back end (server)

## Front end (ui)
front end is the stuff that people see. We have already decided on this in [Part 1](https://shounakshastri.github.io/blogposts/Word%20Prediction%20Model%20-%20PART%201%20-%20Figuring%20out%20the%20problem%20at%20hand) with a very basic drawing made in MSPaint. This to be exact:

![Basic App Layout](/images/ShinyAppLayout.jpg)

So, lets start by writing the ui first:

```r
ui <- fluidPage(
    sidebarLayout(
        sidebarPanel(
            h2("Word Prediction App"),
            h5("1. Select the original text in the text box and start typing. The predictions would appear below."),
            h5("2. The code displays \"...thinking...\" when it is trying to predict a word"),
            h5("3. You can try copying a longer sentence to see what is the prediction."),
            h5("4. The app can process pentagrams i.e. it can predict the 5th word based on the previous 4.")
        ),
        # Show a plot of the generated distribution
        mainPanel(
            
                tabPanel("predict",
                         textInput("user_input", h3("Your Input:"), 
                                   value = "Enter a word or a sentence"),
                         h3("Prediction:"),
                         h4(em(span(textOutput("predicted_word"), style="color:blue"))))
            )
        
    )
)
```
You can see we have made the app fluid. That means, the user doesn't have to do anything other than entering the text in the textbox. The prediction code will run every time the user enters a complete word. 