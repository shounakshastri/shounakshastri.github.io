---
title: "Basic Steganography Concepts"
collection: Steganography
permalink: /Steganography/Basic Steganography Concepts
excerpt: 'This post gives a basic overview of Steganography and is intended towards the uninitiated. We go through some basic techniques and the metrics used to guage the effectiveness of the scheme.'
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


Steganography is used to <span style='color:green'>_hide_</span> data in some other data. Now, in order to understand Steganography, it is important to understand why we hide stuff? Well, we mostly hide stuff because we want it to be safe from others as people might take it or tamper it or even worse, tell everybody else about it. Now, we can hide stacks of cash in a mattress or a can of paint, but what if your stuff is not a stack of cash? What if your stuff is more...incorporeal?

...

*No I am not talking about your feelings!*

Digital stuff! I am talking about digital data! 

What if your stuff is _digital_? Hiding your data or files by creating a bunch of folders in a flash drive kept in your drawer might seem easy, but if someone gets the flash drive, a simple search query would reveal the location of the data. We need something more sophisticated than this.

Enter Steganography.

Steganography hides digital data in other files. This makes it way more difficult to find hidden data. This can be used to hide and share secret information such as watermarks, military and surveillance data, medical records, legal details, the possibilities are ever increasing. 

In this post, we will be using images as the host files to hide secret data. This is because images are known to have redundancies which can be used to hide our secret data.

## Cover and stego files

By now, it should be clear that Steganography needs two files.

1. the file containing the secret data
2. the file in which we would be hiding the secret data - *Cover file/data*

The file which contains the hidden secret data is called the *Stego file*

In more formal terms, if $Emb()$ is your embedding algorithm which helps you embed/hide data in a cover file, then 

$$ stegoFile = Emb(secretData, coverFile, key)$$

where S is the Stego file, and key is the password/auxiliary data required to recover the hidden secret data. The key is usually a biproduct of the embedding process and it's use would be clearer in the upcoming sections.

Similarly, the hidden data can be recovered using an extraction algorithm $Ext()$ given as

$$ secretData = Ext(stegoFile, key)$$

So in short,

$$ secretData = Ext(Emb(secretData, coverFile, key), key)$$

## How to hide data in the cover files?

Images, as mentioned before, contain some inherent redundancies. This coupled with the way the Human Visual System works, makes images great to be used as cover files. Usually, a change of about 20 pixel values is not noticable to humans. Thus, replacing the LSBs of pixels is a great way to hide secret data without really affecting the visual quality of the stgo image.

### Regarding the quality of stego images

It should be noted that stego images, being the carriers of secret data, should not invoke attention of entities which would potentially be looking for the secret data. This is where encryption fails. You can see from the images below that encrypted data/images practically shout that something confidential is being stored or transferred. This attracts hackers who basically take decrypting the data as a challenge. Steganography avoids this by producing high quality images that don't raise any suspicion.

![Stego Image](/images/stego_coloured.jpg){:height="35%" width="35%"} - Stego Image

![Encrypted Data](/images/encrypted_image.jpg) - Encrypted Data

From the above discussion, we can gather that an encryption technique is "broken" when the hacker decrypts the data. But Steganography, due to its nature, is broken when the hacker knows for sure that some data is being hidden or transferred because it failed to pass unnoticed. This is a very important differentiation between Cryptography and Steganography. 

There are several metrics which can be used to check the quality of the stego image, but we will reserve them for a different post. 

## Basic Steganography algorithms

### LSB Replacement

This is the simplest of the stego algorithms. We simply split the cover image into its bitplanes and replace the LSBs by the secret data. At the reciever side, the secret data can be recovered by simply recovering the LSBs of the stego image. The code snipet below shows the embedding  of the secret data using LSB Replacement in MATLAB.

```matlab
cov_img = imread('lena.tif'); % Input cover image
[m, n] = size(cov_img); % Preserve the original size of the cover image
secret_data = randi([0, 1], 1, 10000);

cov_img = cov_img(:);
stg_img = zeros(size(cov_img)); % Initialize stego image 

count = 1;
for ii = 1:length(cov_img)
    stg_img(ii) = bitset(cov_img(ii), 1, secret_data(count)); % LSB Replacement
    count = count + 1;

    if count > length(secret_data) % Break if all secret data is embedded
        stg_img(ii+1:end) = cov_img(ii+1:end);
        break
    end
end

stg_img = reshape(steg_img, m, n) % Reshape the stego image to the original dimensions
```

Higher LSB planes can be used if the size of the secret data is more. The secret data is extracted by simply extracting the LSBs of the stego image.

### Histogram shifting

In histogram shifting, the cover image histogram is analysed and bins which contain peaks and zeros are separated. The histogram between a peak and a zero is shifted to create space for embedding secret data. How? We first scan the histogram of the cover image for the highest peak and the closest zero bin. Then change all the values of the pixels between the peak and the zero by 1 so that the histogram shifts. Now the bin which was a peak is empty. 