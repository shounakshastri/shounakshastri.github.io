---
title: "Basic Steganography Concepts"
collection: Steganography
permalink: /Steganography/Basic Steganography Concepts
excerpt: 'This post gives a basic overview of Steganography and is intended towards the uninitiated. We go through some basic techniques of hiding data in image files with some programming in matlab.'
mathjax: true
tags: 
    - Information Security
    - Steganography
    - Data Hiding
    - matlab    
date: 2020-10-15
---

>This is some spy s***!!!" - A Clash of Clans clanmate


## Introduction


Steganography is used to <span style='color:green'>_hide_</span> data in some other data. Now, in order to understand Steganography, it is important to understand why we hide stuff? Well, we mostly hide stuff because we want it to be safe from others as people might take it or tamper it or even worse, tell everybody else about it. Now, we can hide stacks of cash in a mattress or a can of paint. But what if your stuff is not a stack of cash? What if your stuff is more...incorporeal? Like digital data? Hiding your data or files by creating a bunch of folders in a flash drive kept in your drawer might seem easy, but if someone gets the flash drive, a simple search query would reveal the location of the data. We need something more sophisticated than this.

Enter Steganography.

Steganography hides digital data in other files. This makes it way more difficult to find hidden data. This can be used to hide information such as watermarks, share secret military and surveillance data, medical records, legal details, etc. 

In this post, we will look at the three basic steganography schemes and learn to embed data in images. We use images as the host files to hide secret data because images are known to have redundancies which can be exploited to accomodate our secret data without adding too much distortion to the original files. 

## Cover and stego files

By now, it should be clear that Steganography needs two files.

1. the file containing the secret data
2. the file in which we would be hiding the secret data - *Cover file/data*

The output file which contains the hidden secret data is called the *Stego file*

In more formal terms, if $Emb()$ is your embedding algorithm which helps you embed/hide data in a cover file, then 

$$ stegoFile = Emb(secretData, coverFile, key)$$

where S is the Stego file, and key is the password/auxiliary data required to recover the hidden secret data. The key is usually a biproduct of the embedding process and it's use would be clearer in the upcoming sections.

Similarly, the hidden data can be recovered using an extraction algorithm $Ext()$ given as

$$ secretData = Ext(stegoFile, key)$$

So in short,

$$ secretData = Ext(Emb(secretData, coverFile, key), key)$$

## How to hide data in the cover files?

Images, as mentioned before, contain some inherent redundancies. This coupled with the way the Human Visual System works, makes images great to be used as cover files. Usually, a change of less than 20 pixel values is not noticable to humans. Thus, steganography modifies the cover pixel values to hide secret data while keeping the difference between the cover pixel and the stego pixel to a minimum. 

### Regarding the quality of stego images

It should be noted that stego images, being the carriers of secret data, should not invoke attention of entities which would potentially be looking for the secret data. This is where encryption fails. You can see from the images below that encrypted data/images practically shout that something confidential is being stored or transferred. This attracts hackers who basically take decrypting the data as a challenge. Steganography avoids this by producing high quality images that don't raise any suspicion.

|![](/images/stego_coloured.jpg){:height="35%" width="35%"}|
|:--:|
|*Stego Image*|

|![](/images/encrypted_image.jpg)|
|:--:|
|*Encrypted Data*|

From the above discussion, we can gather that an encryption technique is "broken" when the hacker decrypts the data. But Steganography, due to its nature, is broken when the hacker knows for sure that some data is being hidden or transferred because it failed to pass unnoticed. This is a very important differentiation between Cryptography and Steganography. 

There are several metrics which can be used to check the quality of the stego image, but we will reserve them for a different post. 

## Basic Steganography algorithms

We will be discussing 3 basic techniques here.

1. LSB Replacement
2. Histogram Shifting
3. JPEG Steganography

These techniques form the foundation of all the subsequent advances in Stego algorithms. The code in this segment is written in matlab using the Image Processing Toolbox (IPT). I have seen that many people are more comfortable with Python. The closest alternative to IPT in python is the [opencv package](https://pypi.org/project/opencv-python/). Most of the syntax and functions are the same but with a Python flavour. Such as `i = i + 1` in matlab is `i += 1` in python and `imread()` in matlab becomes `cv2.imread()`. So, translating the code to python would probably not be too complicated. 

Another thing to note at this point is, we will generate the secret data randomly using the internal randomizer of matlab. The reason for this is we usually never embed the secret data directly in the cover image. It is always encrypted first. Because, we need to have a security measure in place to protect our data in case our steganography fails. So, we simulate this by geneating random binary bits and use those as the secret data.

### LSB Replacement

This is the simplest of the stego algorithms. We simply split the cover image into its bitplanes and replace the LSBs by the secret data. At the reciever side, the secret data can be recovered by simply recovering the LSBs of the stego image. The code snipet below shows the embedding  of the secret data using LSB Replacement in MATLAB.

```matlab
cov_img = imread('lena.tif'); % Input cover image
[m, n] = size(cov_img); % Preserve the original size of the cover image
secret_data = randi([0, 1], 1, 10000);

cov_img = cov_img(:);
stg_img = cov_img; % Initialize stego image 

count = 1;
for ii = 1:length(cov_img)
    stg_img(ii) = bitset(cov_img(ii), 1, secret_data(count)); % LSB Replacement
    count = count + 1;

    if count > length(secret_data) % Break if all secret data is embedded
        break
    end
end

stg_img = reshape(steg_img, m, n) % Reshape the stego image to the original dimensions
```

Higher LSB planes can be used if the size of the secret data is more. The secret data is extracted by simply extracting the LSBs of the stego image.

### Histogram shifting

In histogram shifting, the cover image histogram is analysed to locate bins which contain peaks and zeros. A pair of peak-zero bins is chosen and the histogram betweent them is shifted to create space for embedding the secret data.
[The code can be accessed here.](https://github.com/shounakshastri/Histogram-Shifting-RDH)

|![](/images/HIstogram_shifting.jpg){:height="70%" width="70%"}|
|:--:|
|*Histograms of cover and stego images*|

From the above image, it can be seen that one of the peaks from the  original histogram has disappeared. This is where we have hidden the data.
### JPEG Steganography

It is important to discuss JPEG steganography as it is probably the most used image format in the real world. The algorithms which we have discussed before this are all implemented in the spatial domain and can be used to hide data in jpg files just like any other files. But, JPEG uses the Discrete Cosine Transform (DCT) to compress the original uncompressed images. Compression is, in simple words, removal of redundancy. Thus, we have a chance to integrate the data hiding process during the jpg compression. The secret data is hidden in the DCT Coefficients which makes the stego scheme more secure. The data is hidden using the same spatial domain schemes discussed above but the schemes are applied to DCT coefficients instead of raw pixel values. 

---

So, in conclusion, this post gave a brief overview of Steganography. We discussed the possible applications and some other important concepts in relation to cryptography. We also looked at the 3 basic steganography techniques which form the basis of most of the advances in the field. 
