---
title: "OSINT(Exercise 9)"
date: 2025-03-10
description: This is a writeup on OSINT EXERCISE 9 from Sofia Santos' OSINT analysis and exercises.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "credit to Sofia Santos" # Here you can write a small summary of the post if needed
tags: [OSINT]
categories: [Sofia Santos' OSINT exercises]
---
# Exercise 9

This [challenge](https://gralhix.com/list-of-osint-exercises/osint-exercise-009/) was interesting, we are given a video to analyze and answer the questions that follow in the task briefing as shown below.

```bash
The video below was shared by the Visit Tirana Twitter account on February 16, 2023. 

Please answer the following questions:
a) To the best of your knowledge, at what time was the video recorded?
b) Find the coordinates of where the person was walking at the time of the recording.
```

Here is the video [link](https://youtu.be/axC30cE_O-4). and the [tweet](https://x.com/VisitTirana/status/1626342426318077963) as well form twitter

![osintexercise009.png](osintexercise009.png)

## To the best of your knowledge, at what time was the video recorded?

From the tweet itself, we have some interesting information.

1. The name of the photographer  `Eriseld Myrto` 
2. The name of the city where the recording was done `Tirana,Albania` 

with that information we can start by searching if visit tirana has a website which I found using some google dorking as shown below.

![image.png](image.png)

However this did not give me much information, but doing further searches online, I found that the photographer had different alias names on [instagram](https://www.instagram.com/reel/CouwRhAjsQ6/), [tiktok](https://www.tiktok.com/@four_s34sons), [facebook](https://www.facebook.com/53ld1). Form Instagram, we can inspect the source code to get the time when the video was recorded.

![image.png](image%201.png)

From the screenshot above we can only see the date when it was uploaded but when we inspect the source code we get more details such as time as shown below.

![image.png](image%202.png)

now we have the time when the video was uploaded.

`Answer: 2023-02-16T16:48:43.000Z` 

## Find the coordinates of where the person was walking at the time of the recording.

For this section,we shall review the video again taking note of some important features we can use to identify the place easily as shown below.

![image.png](image%203.png)

There is a two identical buildings with one taller than the other, there is a walk path at the middle of the road that seems to be a bike path and pedestrians can also use for walking.

With that lets try and perform a reverse image search and see if we can identify the location of the buildings.

![image.png](image%204.png)

tirana garden building is how the building was identified. We can search that building up on earth pro and see the area.

![image.png](image%205.png)

we have been able to identify the street, we can now play around with street view to see where the person was standing.

![image.png](image%206.png)

Form the above image we have made a side by side analysis and found where the photographer was standing, we can now get the coordinates of the position.

`Answer:41.3270062,19.8072923`