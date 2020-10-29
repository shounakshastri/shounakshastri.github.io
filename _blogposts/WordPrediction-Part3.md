---
title: "Word Prediction Model - PART 3 - Model Building"
collection: blogposts
type: "Word Prediction Model"
permalink: /blogposts/Word Prediction Model - PART 3 - Model Building
tags: 
    - Machine Learning
    - NLP
    - R
    - Predictive Text
    - n-grams
date: 2020-10-20
---

This is the third post in a series which builds a basic word prediction model using R. Here, we will be making the actual model based on the things we learnt in Part 2. The procedure of generating the n-grams is similar to the process we used in the EDA. You can check the other parts from the list given below.

[Part 1 - Figuring out the problem at hand](https://shounakshastri.github.io/NLP/Word%20Prediction%20Model%20-%20PART%201%20-%20Figuring%20out%20the%20problem%20at%20hand)

[Part 2 - Exploring the dataset](https://shounakshastri.github.io/NLP/Word%20Prediction%20Model%20-%20PART%202%20-%20Exploratory%20Data%20Analysis) 

Part 3 - Creating the model

Part 4 - Deploying the model

Part 5 - Conclusion and future scope for improvement

## Introduction

This is PART 3 of our project in which we try to make an application which predicts text. In the previous post, we got our text data and did a basic exploration. We found that the amount of data is quite large and the local machine is not really sufficient in terms of the available RAM. The available RAM runs at 90-95% capacity during the tokenization process. 


Import all the necessary libraries



We will use the same steps as in Part 2 to import the files.


```r
setwd("C:\\Users\\Shounak\\Documents\\Datasets\\Swiftkey")

# Import all the files and open the connection
fileBlogs <- file(".\\final\\en_US\\en_US.blogs.txt", "rb")
fileNews <- file(".\\final\\en_US\\en_US.news.txt", "rb")
fileTweets <- file(".\\final\\en_US\\en_US.twitter.txt", "rb")

# Read the lines and close the connections
blogs <- readLines(fileBlogs, encoding = "UTF-8", skipNul = TRUE)
close(fileBlogs)

news <- readLines(fileNews, encoding = "UTF-8", skipNul = TRUE)
close(fileNews)

tweets <- readLines(fileTweets, encoding = "UTF-8", skipNul = TRUE)
close(fileTweets)

# Remove the variables from the workspace
rm(fileBlogs, fileNews, fileTweets)
```

As seen from the EDA in PART 2, my computer doesn't have enough RAM to handle all the data at once. So, we will break the corpus into samples of a reasonable size and work on that sample, for now.


```r
blogs <- tibble(text = blogs[1:80000])
news <- tibble(text = news[1:80000])
tweets <- tibble(text = tweets[1:80000])
```

Here, I have taken the first 80k entries from the blogs, news and tweets files. I would have to run this program multiple times to generate a model that has been trained on the complete dataset. The main reason for me to take a small sample is my hardware. I am working on a [potato](https://www.urbandictionary.com/define.php?term=Potato%20PC) with 4GB RAM which bottlenecks the CPU and GPU.  *I would recommend that you run this step multiple times to get a good coverage of the data and not miss out on any frequently occurring n-gram phrases.* For me, I can take the rest after I start processing these entries, go get groceries, or catch a movie, cause my computer cannot do anything else.

The pre-processing steps remain the same as that in Part 2, replacing contractions, converting all the letters to lowercase, removing numbers, emoticons, special characters like punctuations, etc and removing extra spaces. 

```r
blogs$text <- replace_contraction(blogs$text)
blogs$text <- tolower(blogs$text)
blogs$text <- gsub("\\d", "", blogs$text) # Remove Numbers
blogs$text <- gsub("[^\x01-\x7F]", "", blogs$text) # Remove emoticons
blogs$text <- gsub("[^[:alnum:]]", " ", blogs$text) # Remove special characters. Adds extra spaces
blogs$text <- gsub("\\s+", " ", blogs$text) # Remove the extra spaces

news$text <- replace_contraction(news$text)
news$text <- tolower(news$text)
news$text <- gsub("\\d", "", news$text) # Remove Numbers
news$text <- gsub("[^\x01-\x7F]", "", news$text) # Remove emoticons
news$text <- gsub("[^[:alnum:]]", " ", news$text) # Remove special characters. Adds extra spaces
news$text <- gsub("\\s+", " ", news$text) # Remove the extra spaces

tweets$text <- replace_contraction(tweets$text)
tweets$text <- tolower(tweets$text)
tweets$text <- gsub("\\d", "", tweets$text) # Remove Numbers
tweets$text <- gsub("[^\x01-\x7F]", "", tweets$text) # Remove emoticons
tweets$text <- gsub("[^[:alnum:]]", " ", tweets$text) # Remove special characters. Adds extra spaces
tweets$text <- gsub("\\s+", " ", tweets$text) # Remove the extra spaces
```

After this step, we simply combine all the three types of data into one table and work on that.

```r
corpus <- rbind(blogs, news, tweets)
rm(blogs, news, tweets)
```

Now, we are going to create the n-gram dictionaries and store them into easily accessible files for deployment. But, since there are a lot of n-grams, we will only keep the ones which occur more frequently. <span style="color:orange"> *I have executed the code for tokenization multiple times before filtering the n-grams to get a better directory. I would advise you the reader to do so too in-case they have limiting hardware.*</span>

The files generated during this process would be uploaded to Shiny along with the code for the application and this make it important to keep the file size small. So, we are going to delete all the n-grams which come up less than 6 times in our sample. But, you can lower the threshold or keep all the n-grams if your hardware or budget supports it. 

### Bigrams

```r
# Creating a list of bigrams

bigrams<- corpus %>%
    unnest_tokens(bigram, text, token = "ngrams", n = 2)

bigramSet <- bigrams %>%
    count(bigram) %>%
    filter(n >= 6)
rm(list = c("bigrams"))

biwords <- bigramSet %>%
    separate(bigram, c("word1", "word2"), sep = " ")
rm(bigramSet)

saveRDS(biwords, "./biwords.rds")
rm(biwords)
```

### Trigrams

```r
# Creating a list of trigrams

trigrams<- corpus %>%
    unnest_tokens(trigram, text, token = "ngrams", n = 3)

trigramSet <- trigrams %>%
    count(trigram) %>%
    filter(n >= 6)
rm(list = c("trigrams"))

triwords <- trigramSet %>%
    separate(trigram, c("word1", "word2", "word3"), sep = " ")
rm(trigramSet)
saveRDS(triwords, "./triwords.rds")
rm(triwords)
```

### Quadgrams

```r
# Creaating a list of quadgrams

quadgrams<- corpus %>%
    unnest_tokens(quadgram, text, token = "ngrams", n = 4)

quadgramSet <- quadgrams %>%
    count(quadgram) %>%
    filter(n >= 6)
rm(list = c("quadgrams"))

quadwords <- quadgramSet %>%
    separate(quadgram, c("word1", "word2", "word3", "word4"), sep = " ")
rm(quadgramSet)
saveRDS(quadwords, "./quadwords.rds")
rm(quadwords)
```
### Pentagrams

```r
# Creating a list of pentagrams

pentagrams<- corpus %>%
    unnest_tokens(pentagram, text, token = "ngrams", n = 5)

pentagramSet <- pentagrams %>%
    count(pentagram) %>%
    filter(n >= 6)
rm(list = c("pentagrams"))
pentawords <- pentagramSet %>%
    separate(pentagram, c("word1", "word2", "word3", "word4", "word5"), sep = " ")
rm(pentagramSet)
saveRDS(pentawords, "./pentawords.rds")
rm(pentawords)

rm(corpus)
gc()
```


## Prediction program

In this section, we will be making four functions, one each for bigrams, trigrams, quadgrams and pentagrams, which would give the last word based on the previous (n-1) words. The code needs to check the input in the text box and predict the next word based on the number of words entered.

The code for word prediction is too long for this post, but [you can check it here.](https://github.com/shounakshastri/Word-Prediction-using-R/blob/main/prediction_code.R)

You can test the code by doing this

```r
input <- "I ate a lot of"
prediction(input)
```

```
[1] "people"
```

Yeah, its not the best. 

This is because the dictionaries that we created don't contain any n-grams that start with the word "ate" but they do contain ones that have "a lot of". So it refers to that phrase "a lot of" and gives the output "people". In this case, the performance would surely be better if we had more data in the files. We can maybe try and reduce the threshold to 4 instead of using 6 and see what happens. It would increase the file size of the directories which would be problematic during the deployment.

In the next part we will create the app and deploy it on Shiny.