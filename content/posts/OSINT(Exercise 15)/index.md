---
title: "OSINT(Exercise 15)"
date: 2025-03-15
description: This is a writeup on OSINT EXERCISE 15 from Sofia Santos' OSINT analysis and exercises.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Sofia Santos" # Here you can write a small summary of the post if needed
tags: [OSINT]
categories: [Sofia Santos' OSINT exercises]
---
# Exercise 15

For this [challenge](https://gralhix.com/list-of-osint-exercises/osint-exercise-015/) we are given a picture giving a description of a telescope we have to find it and locate where it is placed as described in the task briefing below.

```jsx
The image below is a screenshot taken from a CIA declassified 
document. It depicts a caption report of an undisclosed photo taken by 
an agent. The text mentions a telescope “being assembled at factory“.

Your task is to find:

a) A photo that matches the description on the caption report.
b) The exact location of where the telescope was placed once completed.
```

![osint-exercise-015-big-picture.png](osint-exercise-015-big-picture.png)

## A photo that matches the description on the caption report

From the above photo, we can see some clear descriptions of the telescope. The information I used is `telescope for the university of bonn, built at the askania factory, jan 18, 1953` with this information we can search for the telescope on google and see what we shall find as shown below.

![image.png](image.png)

Interesting findings, going to the first [link](https://web.astronomicalheritage.net/show-entity?identity=114&idsubentity=1) and searching through the page, I got an interesting image with some information form our challenge document as shown below.

![image.png](image%201.png)

To verify if this is our telescope, I searched for it and see its elements if they match the description on the document.

![image.png](image%202.png)

As seen above from this [source](https://www.imago-images.com/st/0061895213), we can now verify the telescope we fund matches the description on the document.

![image.png](image%203.png)

Therefore the telescope being identified is as shown below.

![image.png](image%204.png)

## The exact location of where the telescope was placed once completed

From this [site](https://web.astronomicalheritage.net/show-entity?identity=114&idsubentity=1),we got a description of the telescope and where it is located at the `Hoher list observatory` as shown below.

![image.png](image%205.png)

Searching on the same page again for `Tower 1` I got an image of the tower as seen below.

![image.png](image%206.png)

![OHL-Turm1+Schmidt-Spiegel_Wf-0506-n.jpg](OHL-Turm1Schmidt-Spiegel_Wf-0506-n.jpg)

We can search on google maps for the Bonn observatory and see if we can be able to locate the tower with the following patterns as described below.

![image.png](image%207.png)

From google maps we have been able to identify a tower with the surrounding descriptions from our analyzed image above as shown below.

![image.png](image%208.png)

To confirm if it is the tower we can see if there is a street view of the place and see if we can identify if it is truly the tower.

![image.png](image%209.png)

With that evidence we can now clearly identify that our tower is as shown below.

![image.png](image%2010.png)

`Answer: 50°09'42.6"N 6°50'54.0"E`
