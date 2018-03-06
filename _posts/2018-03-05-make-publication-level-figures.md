---
layout: post
title: "Make publication-level figures with free softwares"
author: Wenhu
date: 2018-03-05
mathjax: yes
tags: ImageJ Inkscape
categories: Data_Manipulation
---

* content
{:toc}

> This post documents the process of making figures that satisfy  the rigid publishers. **Repost by indicating the source please!**

## Figures, not pictures

First of all, the figures I mention in this post are not the pictures coming out of scientific softwares, also not shot by equipments like fluorescence microscopy, **they are what you see in the published articles**, the former pics could be considered as the building blocks of figures.

**Most of the contents here are edited from [How to Create Publication-Quality Figures](http://b.nanes.org/figures/index.html)**, I strongly recommend clicking that link, you won't regret!





## The tools

Most of us may feel faint when we shoot a glance at the price tag of photoshop or illustrator, we are just students! What's more, many commercial softwares suck compared to similar open-source buddies. What I will introduce here is two popular, well-documented **free** softwares: 

1. [Inkscape](https://inkscape.org/en/)：typesetting, text annotating, etc. (easy to learn and versatile)

2. [ImageJ](https://imagej.nih.gov/ij/)：format switching, resolution adjustment, etc. It has one distribution, named [Fiji: Fiji is just ImageJ](http://fiji.sc/), which contains lots of plugins, specifically designed for scientific figures analysis.

> Just download them and install!

## Basics

> What is a figure, from a computer's point of view?

Normally, the figures are represented by two kinds of data in computers: **raster data** and **vector data**.

1. Raster data have another name, **bitmap**! For a specific figure, we use grids to separate it into many small squares, named **pixel**, then we define the pixel properties, like shades, colors and so on, all the information are stored in the figure raster data. At last, the computer will piece together the pixels and other metadata to form the overall figure. So, for the same figure, if the number of pixels are higher, the illustrated figure will become finer, this is what we usually say, high **resolution**, and if you enlarge the figure over some extent, you could see the squares, see example below.

2. Vector data are similar to a series of scripting drawing commands which will tell what the computer should do to show the figure on screen, like draw a straight line between A and B, 0.5 pounds, then, draw a circle from B to C, 180 degree, red color, etc. If you enlarge the vector figure, you would not see the pixels, **it is irrelevant to resolution**!

> The publishers usually have one important requirement: the resolution of figures should be no less than ... dpi/ppi! dpi (dots per inch) and ppi (pixels per inch) almost have the same meaning, that is the number of squares/pixels in one unit length (inch here), the more the squares are, the higher the resolution is.

<img src = "http://res.cloudinary.com/dgnsud9ue/image/upload/v1519833016/raster-and-vector.jpg" alt = "万里长城别挡我">

I would not introduce the color representations in computer here, because most of the time we get the ultimate color from the beginning. You could check these links for more information: [Grayscale](https://en.wikipedia.org/wiki/Grayscale), [RGB](https://en.wikipedia.org/wiki/RGB_color_model), [CMYK](https://en.wikipedia.org/wiki/Subtractive_color).

## Step 1: get the pictures (building blocks)

I can't give you a universal way in this step, because different pictures may come out from distinct experiments or softwares.

**The only note you should remember is**: export your pictures as **vector data** as far as possible! In other words, **just export the pictures with extensions: PDF, PostScript or EPS!** Among these, PDF is the most flexible and easy to be pipelined format!

## Step 2: typesetting your pictures -> figures

> **Back up your raw data everytime!**

With a bunch of pictures from step 1, how could we align these pictures in the canvas and annotate them with texts?

**Inkscape coming!**

1. **Import** PDF: `File > Import...`, as many as you want.

2. The key to **typesetting**: `View > Page Grid`, align the pictures based on the grids would be very easy.

3. If you want to group several pictures, just hold `Shift` and use your mouse left button to choose the pictures, then `Ctrl + G` (Group); vice versa, `Ctrl + U` is for ungrouping.

4. **Tailor** your pictures: press `F4`, choose the cutting zone, next, hold `Shift` and click the rest part of the picture, then `Object > Clip > Set`.

5. **Annotating**: `F8`, type what you want to say in the place you like. **tip: Arial or Helvetica fonts are always good looking!**

6. Save: `File > Save As...`, first, save **one .SVG file** for backup, it contains all the information of your figure; second, save **another .PDF file** for possible peer-review requirement.

7. Export: `File > Export PNG Image...`, the current version of Inkscape could only export this PNG format, we could then us ImageJ to switch the format to what we like. As the picture below shows, you could define the dpi value after the `pixels at`. Note the `Page` and `Drawing`, you could try both to see the differences.

<img src = "http://res.cloudinary.com/dgnsud9ue/image/upload/v1519833015/Inkscape-export.jpg" alt = "万里长城别挡我">

## Step 3: format switching

In ImageJ, `File > Open...`, open the PNG file from step 2, then `File > Save As...`, switch to the format you want, like TIFF.

**However**, there is something unusual happening here, if you set the dpi in step 2, like 600, but after you save as TIFF in ImageJ, I found the figure property shows only 96dpi. I finally got a way to fix this, but still don't know the reason:

`File > Open...`, open the PNG file from step 2, then in `Image > Adjust > Scale to DPI`, you should set the dpi again, the default is 600, you could set the value you want, **but note, you need to change the `Width` and `Height` in parallel, for example, you double the default 600 dpi to 1200, then you should shrink the value of `Width` and `Height` in half!** After this, you could safely do `File > Save As...`, switch to the format you need.

## Summary

**1. Use vector data in the beginning as far as possible (like PDF)**

**2. Typesetting and annotating in Inkscape**

**3. Switching DPI and format in ImageJ**


> Please subscribe [RSS](http://bioinfostar.com/feed.xml) if you wanna receive my newest post! 
