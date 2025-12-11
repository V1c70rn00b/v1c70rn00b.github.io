---
title: "OSINT(Exercise 2)"
date: 2025-03-01
description: This is a writeup on OSINT EXERCISE 2 from Sofia Santos' OSINT analysis and exercises.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Sofia Santos" # Here you can write a small summary of the post if needed
tags: [OSINT]
categories: [Sofia Santos' OSINT exercises]
---
# Exercise 2

In this [challenge](https://gralhix.com/list-of-osint-exercises/osint-exercise-002/) we are required to find the name of the train station and the name and height of the tallest building in the image.

```bash
Task briefing: 
The photo below was shared on social media. It clearly depicts a train station.
Please answer the following questions:

a) What is the name of the train station seen in the photo?
b) What is the name and height of the tallest structure seen in the photo?
```

The image we are given is as shown below.

![osintexercise002.png](osintexercise002.png)

However, if you want the entire image you can download it from this [link](https://gralhix.com/wp-content/uploads/2024/09/osint-exercise-002-big-picture.png).

## name of the train station seen in the photo

To find the name of the train station we could use reverse image search or simply search on google maps the name that is clearly seen in the image.

![image.png](image.png)

We shall start with reverse image search where I will be using [google lens](https://lens.google/) for this instance but there are a variety of tools for doing reverse image search such as

1. [Pimeye](https://pimeyes.com/en)
2. [googl lens](https://lens.google/)
3. [Tineye](https://tineye.com/)
4. [yandex](https://yandex.com/)
5. [reveye](https://www.reveye.in/)(This is an extension that can be downloaded on your browser to aid in doing reverse image searches)

just to mention a few.

Using google lens we found the name of the railway station as shown below.

![image.png](image%201.png)

To confirm if that is the real name of the station we could search the name flinders street on google maps or google earth pro and see if we shall get the station.

![Search as of google earth pro.](image%202.png)

Search as of google earth pro.

![Search as of google maps.](image%203.png)

Search as of google maps.

`Answer: Flinders Street Railway Station`

## name and height of the tallest structure seen in the photo

Now for this section we had to find the street view in order to identify the buildings seen on the image and search for their heights online as it is hard to tell the height of a building from an image.

From google earth pro street view we are able to see four buildings that are available but one building is missing.

![Street view analysis from google earth pro.](image%204.png)

Street view analysis from google earth pro.

![The building that is missing from the street view image.](image%205.png)

The building that is missing from the street view image.

However, from the street view image I did some deep google search of the height of the four buildings I identified but two had heights that caught my interest as shown below.

![image.png](image%206.png)

The two buildings **Arts Centre Melbourne of height 162 metres and IBM Australia of height 131 metres, however we could not conclude our search there as there is a building that is lacking form our street view, we could therefore play with the time frames from google earth to see the structure when it was available at the time of the picture being taken.**

After a few minutes of adjusting the time frame to the year 2012, I got a building structure that was not available at our street view as of 2025 as shown below.

![image.png](image%207.png)

As shown below are the structures that were on the image during the capture.

![image.png](image%208.png)

zooming closer, we are able to find the name of the building as shown below.

![image.png](image%209.png)

searching it on google maps as central equity Melbourne as the area is in Melbourne Australia we get this apartment.

![image.png](image%2010.png)

Looking at the street view, it is located behind the two buildings we had identified earlier on the image as shown below and it is indeed the structure that was missing on our earlier street view.

![image.png](image%2011.png)

Searching the height of the building it is 167 metres or 545 ft high as indicated [here](https://www.skyscrapercenter.com/building/focus-melbourne/38852).

Therefore comparing the heights of the three buildings

1. FOCUS Apartments by Central Equity of height 167 metres
2.  **Arts Centre Melbourne of height 162 metres**
3. **IBM Australia of height 131 metres**

we find that the tallest structure is FOCUS Apartments by Central Equity with a height of 167 metres.

`Answer FOCUS Apartments by Central Equity of height 167 metres`