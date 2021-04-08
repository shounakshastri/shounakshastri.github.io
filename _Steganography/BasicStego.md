---
title: "Basic Steganography Concepts"
collection: Steganography
permalink: /Steganography/Basic Steganography Concepts
excerpt: 'This post gives a basic overview of Steganography and is intended towards the uninitiated. We go through some basic techniques of hiding data in image files with some programming in Matlab.'
mathjax: true
tags: 
    - Information Security
    - Steganography
    - Data Hiding
    - Matlab    
date: 2020-10-15
---

>This is some spy s***!!!" - A Clash of Clans clanmate


## Introduction


Steganography is used to <span style='color:green'>_hide_</span> data in some other data. Now, in order to understand Steganography, it is important to understand why we hide stuff? Well, we mostly hide stuff because we want it to be safe from others as people might take it or tamper it or even worse, tell everybody else about it. Now, we can hide stacks of cash in a mattress or a can of paint. But what if your stuff is not a stack of cash? What if your stuff is more...intangible? Like digital data? Hiding your data or files by creating a bunch of folders in a flash drive kept in your drawer might seem easy, but if someone gets the flash drive, a simple search query would reveal the location of the data. We need something more sophisticated than this.

Enter Steganography.

Steganography hides digital data in other files. This makes it way more difficult to find hidden data. This can be used to hide information such as watermarks, share secret military and surveillance data, medical records, legal details, etc. 

In this post, we will look at the three basic steganography schemes and learn to embed data in images. We use images as the host files to hide secret data because images are known to have redundancies which can be exploited to accommodate our secret data without adding too much distortion to the original files. 

## Cover and stego files

By now, it should be clear that Steganography needs two files.

1. the file containing the secret data
2. the file in which we would be hiding the secret data - *Cover file/data*

The output file which contains the hidden secret data is called the *Stego file*

Now, if $Emb()$ is your embedding algorithm which helps you embed/hide data in a cover file, then the stego file can be given as

$$ stegoFile = Emb(secretData, coverFile, key)$$

where key is the password/auxiliary data required to recover the hidden secret data. The key is usually a by-product of the embedding process and it's use would be clearer in the upcoming sections.

Similarly, the hidden data can be recovered using an extraction algorithm $Ext()$ given as

$$ secretData = Ext(stegoFile, key)$$

So in short,

$$ secretData = Ext(Emb(secretData, coverFile, key), key)$$

## How to hide data in the cover files?

Images, as mentioned before, contain some inherent redundancies. This coupled with the way the Human Visual System works, makes images great to be used as cover files. Usually, a change of less than 20 pixel values is not noticeable to humans. Thus, steganography modifies the cover pixel values while keeping the difference between the cover pixel and the stego pixel to a minimum. 

### Regarding the quality of stego images

It should be noted that stego images, being the carriers of secret data, should not invoke attention of entities which would potentially be looking for the secret data. This is where encryption fails. You can see from the images below that encrypted data/images practically shout that something confidential is being stored or transferred. This attracts hackers who basically take decrypting the data as a challenge. Steganography avoids this by producing high quality images that don't raise any suspicion.

|![](/images/stego_coloured.jpg){:height="35%" width="35%"}|
|:--:|
|*Stego Image*|

|![](/images/encrypted_image.jpg)|
|:--:|
|*Encrypted Data*|

From the above discussion, we can gather that an encryption technique is "broken" when the hacker decrypts the data. But Steganography, due to its nature, is broken when the hacker knows for sure that some data is hidden or being transferred because it failed to pass unnoticed. This is a very important differentiation between Cryptography and Steganography. 

There are several metrics which can be used to check the quality of the stego image, but we will reserve them for a different post. 

## Basic Steganography algorithms

We will be discussing 3 basic techniques here.

1. LSB Replacement
2. Histogram Shifting
3. JPEG Steganography

These techniques form the foundation of all the subsequent advances in Stego algorithms. The code in this segment is written in Matlab using the Image Processing Toolbox (IPT). I have seen that many people are more comfortable with Python. The closest alternative to IPT in Python is the [OpenCV package](https://pypi.org/project/opencv-python/). Also, using Numpy would also make it easier to handle matrices. Most of the syntax and functions are the same but with a Python flavour. Such as `i = i + 1` in Matlab can be written as `i += 1` in Python and `imread()` in Matlab becomes `cv2.imread()` when using OpenCV. So, translating the code to python should be quite straightforward.

Another thing to note at this point is, we will generate the secret data randomly using the internal randomizer of Matlab. The reason for this is we usually never embed the secret data directly in the cover image. It is always encrypted first. This is because we need to have a security measure in place to protect our data in case the steganography fails. So, we simulate this by generating random binary bits and use those as the secret data.

### LSB Replacement

This is the simplest of the stego algorithms. We split the cover image into its bit planes and replace the LSBs by the secret data. At the receiver side, the secret data can be recovered by simply recovering the LSBs of the stego image. The code snippet below shows the embedding  of the secret data using LSB Replacement in MATLAB.

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

Higher LSB planes can be used if the size of the secret data is more. The secret data is extracted by simply extracting the LSBs of the stego image. You might have noticed that the original LSBs have been discarded. Thus, the original image cannot be recovered. If it is important to recover the original image, then a separate file with the original LSBs has to be sent through a secure channel. But, this poses a question that breaks LSB replacement as a method with reversibility. The question is that, if there is a secure channel available which can send data to the receiver without leaking it to third parties, why should we bother to hide the data in the first place? We can directly send our secret data on the secure channel and be done with it. This is one of the main factors which has led to further research in LSB based data hiding techniques.

### Histogram shifting

In histogram shifting, the cover image histogram is analysed to locate bins which contain peaks and zeros. A pair of peak-zero bins is chosen and the histogram between them is shifted to create space for embedding the secret data. The length of the secret data and the peak and zero pair is sent to the receiver as auxiliary data. At the receiver, the image is scanned again and every time a pixel with either P or P $\pm$ 1 is found, we either extract a 0 or a 1 based on the auxiliary data. [The code for embedding and extraction can be accessed here.](https://github.com/shounakshastri/Histogram-Shifting-RDH)

|![](/images/HIstogram_shifting.jpg)|
|:--:|
|*Histograms of cover and stego images*|

From the above image, it can be seen that one of the peaks from the  original histogram has disappeared. This is where we have embedded hidden the data. If you analyse the code, it will be evident that the number of bits that can be hidden in any given image are limited by the number of pixels corresponding to peaks and zeros in the histogram. It is possible that a cover image might not have any zeros at all. This is where the algorithm fails. You have to be careful about which image you choose as a cover. The image should have zero bins and enough pixels in the peaks to fully accommodate the secret data.

One more thing to note here is that it is possible to recover the original cover image. Although it is not implemented in the code linked above, it is simple enough that anybody with a minimal coding experience can add the lines for recovery of the original image.


### JPEG Steganography

It is important to discuss JPEG steganography as it is probably the most used image format in the real world. The algorithms which we have discussed before this are all implemented in the spatial domain and can be used to hide data in jpg files just like any other files. But, JPEG uses the Discrete Cosine Transform (DCT) to compress the original uncompressed images. Compression is, in simple words, removal of redundancy. Thus, we have a chance to integrate the data hiding process during the jpg compression. The secret data is hidden in the DCT Coefficients which makes the stego scheme more secure. The data is hidden using the same spatial domain schemes discussed above but the schemes are applied to DCT coefficients instead of raw pixel values. 

---

So, in conclusion, this post gave a brief overview of Steganography. We discussed the possible applications and some other important concepts in relation to cryptography. We also looked at the 3 basic steganography techniques which form the basis of most of the advances in the field. 
