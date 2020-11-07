---
title: "Metrics used in image cryptography"
collection: blogposts
type: blogposts
excerpt: This post gives an intuition about the metrics being used in academic literature on image encryption. 
permalink: /blogposts/Word Prediction Model - PART 1 - Figuring out the problem at hand
tags: 
    - Encryption
    - Image Processing
    - npcr
    - uaci
    - Key space analysis
    - correlation
    - histogram 

date: 2020-10-13
---
During my time as a Teaching Assistant, my PI used to send his project students to me to get their basics straight. One of their main difficulties would be that they couldn't understand the metrics used while testing their encryption schemes. In this blog post, I am going to explain some of the most commonly used image encryption metrics in academic literature.

If you read any recent academic literature based on image encryption, chances are you might come across the following terms

1. Adjacent pixel correlation
2. NPCR
3. UACI
4. Histogram analysis
5. Robustness
6. Key space analysis

We will go through these terms one-by-one and try to develop some intuition behind them.

## 1. Adjacent pixel correlation

In image processing, correlation is used for matching templates. ([template matching resource](https://www2.cs.duke.edu/courses/fall15/compsci527/notes/convolution-filtering.pdf)) This can be transferred to encryption to check if any part of the encrypted image is similar to a given template. 

Usually, academic literature provides results of pixel correlation using either tables or scatter plots (sometimes they give both which makes no sense). I, personally, prefer tables because they reduce the number of pages which can be a big problem while writing academic papers. The figures look something like this:

|![](/images/HO.png){:height="50%" width="50%"}|
|:--:|
|*Horizontal correlation for the plain image*|

|![](/images/HE.png){:height="50%" width="50%"}|
|:--:|
|*Horizontal correlation for the encrypted image*|

These plots are created by selecting around 2000 pairs of horizontally adjacent pixels in the original and encrypted images. The correlation is directly calculated between the pixel pairs using the inbuilt functions in the tool you are using (most probably Matlab).  
If the correlation is near 0 it means the pixels are not correlated and the encryption has been effective in removing the correlation. The usual values would be either positive or negative but nearly zero.
