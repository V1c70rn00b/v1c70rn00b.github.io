---
title: "OSINT(Exercise 7)"
date: 2025-03-06
description: This is a writeup on OSINT EXERCISE 7 from Sofia Santos' OSINT analysis and exercises.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Sofia Santos" # Here you can write a small summary of the post if needed
tags: [OSINT]
categories: [Sofia Santos' OSINT exercises]
---
# Exercise 7

This [challenge](https://gralhix.com/list-of-osint-exercises/osint-exercise-007/) was fun to do, below is the task briefing.

```bash
The photo below was taken a few years ago in a beautiful city.

Your task is to find the answers to the following questions:
a) Where was the photo taken?
b) In which year was the photo taken?
c) The big poster on the right contained a link to a website. What was the link?
```

![osintexercise007.png](osintexercise007.png)

to get the full image you can access it from this [link](https://gralhix.com/wp-content/uploads/2023/08/osint-exercise-007-big-picture.png).

## Where was the photo taken?

For this section, I did a reverse image search using google lens and found the name of the location as shown below.

![image.png](image.png)

From the above search we now know the location is lisbon where there is a mall called vasco da gama, but wait with [history](https://www.britannica.com/biography/Vasco-da-Gama) I know vasco da gama was a Portuguese explorer, with that we can now head to google maps and search for vasco da gama mall in Lisbon, Portugal and see where the image was taken as shown below.

![image.png](image%201.png)

As seen above the structure in our image is right in front of the vasco da gamma mall, from the image challenge, the photo was taken from the top of the escalator. The entire place is called **Parque das Nações.**

`Answer: **Parque das Nações, Lisbon Portugal`** 

## which year was the photo taken and What was the link ?

For this section, we had to find the poster in the image in which it is an event advert form there we shall get the year when the photo was taken as event adverts are usually dated when they will happen. 

We shall play around with google earth pro street view and time adjustments to see if we can find the poster from the image as shown in the challenge photo and probably find the link in the poster as well.

![image.png](image%202.png)

We have been able to identify where the poster was placed on the wall. Tried to play around with the street on google earth pro but I was unable to find the year of the poster, I decided to look up closely on the image and try to read the readings on the poster by cropping the image to have the poster alone and perform a reverse image search for the poster as shown below.

![image.png](image%203.png)

the image of the poster cropped from the main picture.

![image.png](image%204.png)

Performing reverse image search, I was able to get the name of the event and write-ups too as well. Now the event is `Tutakhamon` this is according to venice searching on google for the same event that happened on Portugal I found the the following.

![image.png](image%205.png)

The event was called tutankamon in Portuguese. Searching again for the event, I found the exact poster as shown below.

![image.png](image%206.png)

Following the F[acebook post](https://web.facebook.com/1057516927623387/photos/pb.100064532476679.-2207520000/2772725619435834/?type=3), I was able to find the year and link on the poster as shown below.

![58775370_2772725622769167_8102530527745015808_n.jpg](58775370_2772725622769167_8102530527745015808_n.jpg)

`Answer: year=2019, link= www.tutankamon.pt`