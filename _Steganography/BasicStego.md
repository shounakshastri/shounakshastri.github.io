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

## Introduction

> "This is some spy s***!!!"
>               - A Clash of Clans clanmate

This is usually the reaction when I tell people about Steganography. The younger ones atleast. The older people usually say "Well, thats all fine and dandy. But, what are you going to do with this?". Thus, Steganography tends to generate two extreme opinions, 

1. that it's cool 
2. this person has wasted 5 years of his life

So, Steganography is used to <span style='color:green'>_hide_</span> data in some other data. Now, in order to understand Steganography, it is important to understand why we hide stuff? Well, mostly because we want the said stuff to be safe from others as people might take it or tamper it or even worse, tell everybody else about it. Now, we can hide stacks of cash in a mattress or a can of paint, but what if your stuff is not a stack of cash? What if your stuff is more...incorporeal?

...

*No I am not talking about your feelings!*

Digital stuff! I am taling about digital data! 

What if your stuff is _digital_? Hiding your data or files by creating a bunch of folders in a flash drive kept in your drawer might seem easy, but if someone gets the flash drive, a simple search query would give the location of the data. We need something more sophisticated than this.

Enter Steganography.

Steganography hides digital data in other files. This makes it way more difficult to find hidden data. This can be used to hide secret military information like serveillance data in, or medical records, legal details, the possibilities are ever increasing.

## Cover and stego files

By now, it should be clear that Steganography needs two files.

1. the file containing the secret data
2. the file in which we would be hiding the secret data - *Cover file/data*

The file which contains the hidden secret data is called the *Stego file*

Mathematically speaking, if 

When \(a \ne 0\), there are two solutions to \(ax^2 + bx + c = 0\) and they are
$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$