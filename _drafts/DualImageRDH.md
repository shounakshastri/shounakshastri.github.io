---
title: "Dual Image Reversible Data Hiding"
collection: Steganography
permalink: /Steganography/Dual Image Reversible Data Hiding
excerpt: 'In this post, I have discussed the concepts related to Dual Image Reversible Data Hiding (DIRDH), a subject on which a considerable part of my PhD is based. We look at some basic concepts and algorithms while highlighting how these schemes are different when compared to the basic RDH schemes.'
mathjax: true
tags: 
    - Authentication
    - Image Processing
    - Information Security
    - Steganography
    - Data Hiding
    - Matlab
date: 2020-11-30
---

_This post needs some background about Steganography. You can check out my [previous post]() to get a gist._

## Introduction to DIRDH

As we know from this [previous post](), Data Hiding is used to embed secret data in a cover image. In our previous post, we have seen that data hiding produces one stego image which contains the hidden secret data. In this post we will discuss about Dual Image Reversible Data Hiding or DIRDH algorithms which generate two stego images instead of one.

A stego algorithm needs to have the following characteristics to be classified as a DIRDH algorithm:

1. It should produce 2 stego images.
2. The secret data is only recoverable when both stego images are available.
3. The original cover image is recoverable.

These algorithms can be used in traditional stego applications discussed in our previous post. But in addition to those, these can be used to authenticate users. The fact data cannot be recovered unless both stego images are present can be used to compare the secret data from the two images and verify the users identity. This would be more clear as we discuss the DIRDH schemes.

## Embedding tables based DIRDH schemes
These schemes have a set of rules 


