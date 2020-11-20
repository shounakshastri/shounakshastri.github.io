---
title: "Metrics used in image cryptography"
collection: blogposts
type: blogposts
excerpt: This post gives an intuition about the metrics being used in academic literature on image encryption. 
permalink: /blogposts/Metrics used in image cryptography
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
2. Number of Pixel Change Ratio (NPCR) and Unified Average Change Intensity (UACI)
3. Histogram analysis
4. Robustness
5. Key space analysis

We will go through these terms one-by-one and try to develop some intuition behind them.

## 1. Adjacent pixel correlation

In image processing, correlation is used for matching templates ([template matching resource](https://www2.cs.duke.edu/courses/fall15/compsci527/notes/convolution-filtering.pdf)). This can be transferred to encryption to check if any part of the encrypted image is similar to a given template. How? Well, correlation between adjacent pixels can be used to detect patterns. A smaller size of the template would mean the check is more rigorous.

Usually, academic literature provides results of pixel correlation using either tables or scatter plots (sometimes they give both which makes no sense). I, personally, prefer tables because they reduce the number of pages which can be a big problem while writing academic papers. The figures look something like this:

|![](/images/HO.png){:height="50%" width="50%"}|
|:--:|
|*Horizontal correlation for the plain image*|

|![](/images/HE.png){:height="50%" width="50%"}|
|:--:|
|*Horizontal correlation for the encrypted image*|

These plots are created by selecting around 2000 pairs of horizontally adjacent pixels in the original and encrypted images. The correlation between the pixels can be directly calculated using the inbuilt functions in the tool you are using (most probably Matlab or OpenCV). If the correlation is near 0 it means the pixels are not correlated and the encryption has been effective in removing the correlation. The values are between -1 and 1. In the case of encryption, we usually ignore the sign. If the correlation is close to 0, it means the two images are not correlated and the encryption has been successful. 

## 2. Number of Pixel Change Ratio (NPCR) and Unified Average Change Intensity (UACI)

These metrics are used to check the resistance of the encryption technique to differential attacks. Differential attacks tell us how changes in the plain text affect the cipher text. Thus, NPCR and UACI track if a minor change in the original pixel value propagates or gets amplified in the encrypted image. To calculate this metric, we have to perform the encryption process twice. Once on the original plain image, and next on an image with a change of 1-bit in a random pixel.

NPCR and UACI are calculated using the following equations

$$ D(i, j) = \begin{cases}
                    0, & \text {if } C^1(i, j) = C^2(i, j) \\
                    1, & \text {if } C^1(i, j) \neq C^2(i, j)  
             \end{cases} $$          

$$ NPCR = \sum_{i, j} \frac {D(i, j)}{T} $$

$$ UACI =  \sum_{i, j} \frac {C^1(i, j) - C^2(i, j)}{F \times T} $$