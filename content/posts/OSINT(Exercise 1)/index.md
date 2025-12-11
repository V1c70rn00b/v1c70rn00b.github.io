---
title: "OSINT(Exercise 1)"
date: 2025-03-01
description: This is a writeup on OSINT EXERCISE 1 from Sofia Santos' OSINT analysis and exercises.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Sofia Santos" # Here you can write a small summary of the post if needed
tags: [OSINT]
categories: [Sofia Santos' OSINT exercises]
---

# Exercise 1

In this [challenge](https://gralhix.com/list-of-osint-exercises/osint-exercise-001/) we shall be getting the coordinates of where the photo was taken as shown in the description.

```bash
Task briefing: 
Below you can see a screenshot from a tweet containing a photo. It contains all the relevant information 
necessary to help you find the exact location. 
Please identify the coordinates of where the photo was taken.
```

We are given a photo form twitter to get the coordinates of the image

![osintexercise001.png](osintexercise001.png)

However, to get the image alone we can download it form this [link](https://gralhix.com/wp-content/uploads/2023/08/osint-exercise-001-big-picture.jpeg). We shall start by analyzing the image first, furthermore, from the tweet, it seems the image was taken from a town called Kiffa. Doing some search on line, the city is located in Mauritania.

![image.png](image.png)

At least now we have a small geographical area to conduct our search of the image but lets first analyze the image.

![image.png](image%201.png)

As shown above we have analyzed the image, and now we can play around with google earth pro to identify the exact coordinates of the image. To add, the image shows clearly the roads are tarmacked as for many roads in semi-arid regions are usually murram roads and the photo was taken on 2013 february as palces do develop, we shall need to adjust the time frame to 2013 as shown below.

![image.png](image%202.png)

With that set we can now try and find from the map areas around kiffa where there are tarmac roads to easily identify our location. After some few minutes of combing through the map, I found the location of where the image was taken as shown below.

![image.png](image%203.png)

The coordinates of the area were 16°36'33"N 11°23'52"W. Moreover, the tweet said “present themselves to the newcomers” looking at the area ahead, there is an airport, the newcomers being referred to could be the tourist.

![image.png](image%204.png)

This challenge was quite interesting. The tools used are [google earth pro](https://earth.google.com) (I know it is pro but it is not paid for, it is absolutely free.).

> To find the time when the image was taken one could use the [shademap](https://shademap.app/) tool to look at the shadow formation by adjusting the time, to understand on how to solve the challenge using the tool you could read this [writeup](https://mugr.at/walkthroughs/sofias-osint-001/) by [notmugrat](https://x.com/notmugrat).
>