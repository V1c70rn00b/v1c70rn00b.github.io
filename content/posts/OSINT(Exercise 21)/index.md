---
title: "OSINT(Exercise 21)"
date: 2025-03-21
description: This is a writeup on OSINT EXERCISE 21 from Sofia Santos' OSINT analysis and exercises.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Sofia Santos" # Here you can write a small summary of the post if needed
tags: [OSINT]
categories: [Sofia Santos' OSINT exercises]
---
# Exercise 21

For this [challenge](https://gralhix.com/list-of-osint-exercises/osint-exercise-021/)we are required to find the location on the chocolate as stated in the task briefing below.
```
Maps can appear in the most unusual places. The figure below contains a chocolate bar featuring a map. Next to it you will find the satellite view of the same location.

Your task is to:  
a) Find the coordinates of the location seen in both images.
```

![SOURCE](osint-exercise-021-big-picture.png)

## The coordinates of the location
For this section I begun by searching the words on the logo in the chocolate which were `DO IT RIGHT` as seen on the image of the chocolate.
![](Pasted%20image%2020250321152345.png)
The chocolate is called Puchero, we can now go to their [page](https://somospuchero.com/en/category-product/chocolate-en/) and see more details about it.
![](Pasted%20image%2020250321152613.png)
There are more chocolates form the company, however, when we navigate to the `GET IN TOUCH` page, we are met with a map that we can analyze further.
![](Pasted%20image%2020250321152830.png)
From the map, it is almost resembling the map on the chocolate, we shall copy the location and open it on google earth pro and look at it closer with the chocolate image on the side and compare if they truly are the same.
![](Pasted%20image%2020250321153553.png)
From the analysis above we have identified the location on the chocolate was the actual map of where the chocolate company is located.

`Answer: 41°21'10.3"N 4°41'24.4"W`