---
title: "OSINT(Exercise 22)"
date: 2025-03-24
description: This is a writeup on OSINT EXERCISE 22 from Sofia Santos' OSINT analysis and exercises.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Sofia Santos" # Here you can write a small summary of the post if neededs
tags: [OSINT]
categories: [Sofia Santos' OSINT exercises]
---
# Exercise 22
For this [challenge](https://gralhix.com/list-of-osint-exercises/osint-exercise-022/), we are required to find a partial number plate of the trucks and where the camera is located as indicated in the task briefing below.
```
The two images below come from a security camera overseeing a port. They were taken almost nine years apart. Both have a vehicle highlighted.

Your task is to:  
a) Find the link to the camera’s live feed.
b) Geolocate the security camera.  
c) Find the license plate of the vehicle highlighted in blue.  
d) Find the licence plate of the vehicle highlighted in orange.

Note:For the sake of privacy you only need to provide the bottom half of the licence plate containing 2 sets of numeric values, as such: [XX]-[XX].
```
Below are the images we are required to use for or analysis.
![](osintexercise022-a.png)
![](osintexercise022-b.png)

## Find the link to the camera’s live feed
From the first photo we can identify some details as shown below.

![](Pasted%20image%2020250324121423.png)

The camera live feed is from Yamoto fishing port and it was on `March 11, 2015`.Although we have been able to identify the port at which the live feed was from, the information is not enough for us to use to get the link of the camera live feed. We can perform a reverse image search and see if we can get more information. After a couple of searches and looking at the note on the challenge `* Note: Although it is still possible to find the link, unfortunately, the live feed has been down since January 2025`, I was unable to find the live feed but from past participants of this challenge the link that was found to contain the live feed was this [link](http://202.239.224.34/cgi-bin/guestimage.html).

`Answer: http://202.239.224.34/cgi-bin/guestimage.html`

## Geolocate the security camera.
For this section, we need to search for the port on google maps and try to identify the street view in order to find the view that looks similar to that in the captured video.

![](Pasted%20image%2020250324124819.png)

As seen above, after analyzing the image and the street view, we are able to identify where the camera is. With further analysis, we area able to identify the location of the camera by zooming in on the street view as shown below.

![](Pasted%20image%2020250324125028.png)

The location of the camera is `38°24'34.1"N 141°14'40.6"E`.

![](Pasted%20image%2020250324125543.png)

`Answer: 38°24'34.1"N 141°14'40.6"E`

## Find the license plate of the vehicle highlighted in blue
We are required to identify the number plate of the truck in this image below.

![](osintexercise022-a%201.png)

From the street view, we are unable to identify the vehicle in blue rather the truck in orange, however, form the structure of the truck it seems they are both from the same company, looking at the street view in google maps, I was able to identify the name of the company ` Aizawa suisan Japan` and searched through their website where I was able to identify a truck that is a replica of the one in blue as shown below.

![](Pasted%20image%2020250324132737.png)

The number plate of the truck is `56-36`

`Answer:56-36`

## Find the license plate of the vehicle highlighted in orange.
The image of the vehicle we are required to identify on the image is as shown below.

![](osintexercise022-b%201.png)

This vehicle was easy to identify on google maps as shown below.

![](Pasted%20image%2020250324130808.png)

From the body of the truck, we can identify the details of the company as `Aizawa suisan Japan`.

![](Pasted%20image%2020250324131110.png)

We can search for this company and see if we can be able to identify the number plate of the vehicle if they have it on their website. However, we are unable to find the exact image of the truck. I searched for the ;ink to any of their social media pages and found for facebook from this [page](https://en.machindo-higamatsu.com/aizawa-suisan)  as shown below.

![](Pasted%20image%2020250324133703.png)

the number plate is `31-20`

`Answer: 31-20`
