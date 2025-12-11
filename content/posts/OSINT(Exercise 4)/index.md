---
title: "OSINT(Exercise 4)"
date: 2025-03-04
description: This is a writeup on OSINT EXERCISE 4 from Sofia Santos' OSINT analysis and exercises.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Sofia Santos" # Here you can write a small summary of the post if needed
tags: [OSINT]
categories: [Sofia Santos' OSINT exercises]
---
# Exercise 4

This [challenge](https://gralhix.com/list-of-osint-exercises/osint-exercise-004/) was quite easy as seen below are the tasks briefings.

```bash
Task briefing: 
This is a photo of a resort located on an island. 
a) What is the name of the resort?
b) What are the coordinates of the island?
c) In which cardinal direction was the camera facing when the photo was taken?
```

We can access the image from this [link](https://gralhix.com/wp-content/uploads/2023/08/osint-exercise-004-big-picture.jpg).

![osint-exercise-004-big-picture.jpg](osint-exercise-004-big-picture.jpg)

## The name of the resort

For this section we could do a reverse image search to get the name of the resort, for me I used this [extension](https://reveye-reverse-image-search.en.softonic.com/chrome/extension) that allows you to search for the image using image searchers such as [yandex](https://yandex.com),[TinEye,](https://www.tineye.com) [bing](https://www.bing.com) and [google lens](https://www.google.com)

![image.png](image.png)

However,going through the various searches found in the different search engines, google lens and Bing were the only one that gave me the exact image that I was looking for as shown below.

![Results as per google lens.](image%201.png)

Results as per google lens.

![Results as per Bing search.](image%202.png)

Results as per Bing search.

As seen above Bing was also able to find a write-up done by another person but I preferred focusing on finding the results on my own. As for google lens, we were able to find the image and we found a website for the resort containing the image as the background of the website as shown below.

![image.png](image%203.png)

`Answer: Oan Resort`

## The coordinates of the island.

For this section we are required to search for the co-ordinates of the resort, this was easy as we could search the resort straight from google maps and get the exact coordinates of the island as sown below.

![image.png](image%204.png)

`Answer: 7°21'49.0"N 151°45'22.4"E` 

## Cardinal direction  the camera was facing when the photo was taken.

For this section we shall play around with google earth pro using the coordinates from google maps and put the image along side the window for google earth pro to get the exact cardinal direction the camera was facing as shown below.

![image.png](image%205.png)

As shown above, we have found the exact cardinal direction from which the camera was facing when the picture was taken.

`Answer: North West`