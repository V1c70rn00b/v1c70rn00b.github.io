---
title: "Clouded Perception"
date: 2026-03-09
description: This is a writeup on Bellingcat OSINT challenge series of Maritime Mysteries
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Bellingcat" # Here you can write a small summary of the post if needed
series: [Maritime Mysteries]
series_oredr: 4
tags: [OSINT]
categories: [Bellingcat]
---
# Clouded Perception

Challenge description

```jsx
This serene lake is a beautiful sight, but something is off: this image has been manipulated.
What should we do if an image is altered, either by hand or with AI?

What is the 6 digit number hidden in the image?
```

The image can be accessed from the following [link](https://challenge.bellingcat.com/assets/manipulated_lake-By7pLY1L.jpg)

This challenge was an interesting one, first I checked the exif data of the image but found no worthy details. I decided to use an online tool called [forensically](https://29a.ch/photo-forensics/#forensic-magnifier) to check the image for further details by playing around with the color alteration of the image and found the hidden number as shown below.

![image.png](image.png)

Thanks to taking part in a number of CTFs, I was able to find this tool quite easily for image analysis especially from the digital forensics view point.

Answer: `428309`